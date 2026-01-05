# Signal NFX - Investor Intelligence Platform

## ¿Qué es Signal?

[Signal](https://signal.nfx.com/) es una herramienta **gratuita** desarrollada por [NFX](https://www.nfx.com/) que funciona como una red de inteligencia para el ecosistema de inversión. Conecta fundadores, capitalistas de riesgo (VCs), scouts y ángeles inversionistas.

### Características Principales

| Característica | Descripción |
|----------------|-------------|
| **Precio** | Gratuito (cuenta free disponible) |
| **Tipo de Datos** | Perfiles de inversores, historial de inversiones, firmas de VC |
| **Acceso** | Web UI + API GraphQL (no documentada oficialmente) |
| **Cobertura** | Global, enfocado en ecosistema VC/startup |

### Propósito Original

- Ayudar a **fundadores** a encontrar inversores adecuados según sector, etapa y calidad
- Permitir a **inversores** conectarse y colaborar
- Mapear redes de co-inversión y relaciones del ecosistema

---

## Relevancia para Kauffman Fellows

Signal proporciona datos de **Investment Data** que complementan o podrían reemplazar a Crunchbase:

| Dato Disponible | Relevancia | Campo Kauffman |
|-----------------|------------|----------------|
| Perfil de inversor | Alta | Job Title, Current Firm |
| Historial de inversiones | Alta | Portfolio tracking |
| Rondas de funding | Alta | Investment activity |
| Co-inversores | Alta | Network mapping |
| Etapas de inversión | Alta | Preferred Stages |
| Áreas de interés | Alta | Investing Thesis |
| Rango de inversión (min/max/target) | Alta | Investment focus |
| URLs sociales (LinkedIn, Twitter, Crunchbase) | Alta | Contact enrichment |

### Ventajas sobre Crunchbase

1. **Gratis** - No requiere suscripción enterprise
2. **Datos específicos de inversores** - Enfocado en VCs, no solo companies
3. **API GraphQL accesible** - Permite automatización
4. **Perfiles públicos** - Acceso directo via URL sin autenticación

---

## Métodos de Acceso a Datos

### 1. Acceso Directo via URL (Sin Cuenta)

Los perfiles de inversores son accesibles públicamente:

```
https://signal.nfx.com/investors/{slug}
```

**Ejemplos:**
- https://signal.nfx.com/investors/allen-taylor
- https://signal.nfx.com/investors/andy-mcloughlin

El `slug` es el nombre completo en formato kebab-case (`first-name-last-name`).

### 2. API GraphQL (Requiere Cuenta)

```
Endpoint: https://signal-api.nfx.com/graphql
Method: POST
Headers:
  - Content-Type: application/json
  - Authorization: Bearer {TOKEN}
```

> **Nota**: El token se obtiene al autenticarse en signal.nfx.com

---

## Queries Disponibles

### Autocomplete de Inversores

Buscar inversores por nombre o firma:

```graphql
query InvestorsAutocompleteQuery($name_or_firm: String, $first: Int) {
  investors(name_or_firm: $name_or_firm, first: $first) {
    edges {
      node {
        id
        person {
          id
          slug
          name
          first_name
          last_name
        }
        firm {
          id
          name
        }
        image_urls
      }
    }
  }
  firms(search_name: $name_or_firm, first: $first) {
    edges {
      node {
        name
        slug
        id
      }
    }
  }
}
```

Ver ejemplo completo: [examples/autocomplete-query.json](examples/autocomplete-query.json)

### Perfil Completo de Inversor

Obtener todos los datos de un inversor:

```graphql
query InvestorProfileLoad($personId: ID!) {
  investor_profile(person_id: $personId) {
    id
    person { ... }
    firm { ... }
    stages { ... }
    positions { ... }
    investments_on_record { ... }
    # ... más campos
  }
}
```

Ver ejemplo completo: [examples/investor-profile-query.json](examples/investor-profile-query.json)

---

## Datos Disponibles por Perfil

### Información Personal

| Campo | Tipo | Ejemplo |
|-------|------|---------|
| `name` | String | "Allen Taylor" |
| `first_name` | String | "Allen" |
| `last_name` | String | "Taylor" |
| `linkedin_url` | String | "https://www.linkedin.com/in/aktaylor/" |
| `twitter_url` | String | "https://www.twitter.com/aktaylor" |
| `crunchbase_url` | String | "https://www.crunchbase.com/person/allen-taylor" |
| `angellist_url` | String | "https://www.angel.co/p/allen-taylor" |
| `url` | String | URL personal/empresa |
| `roles` | Array | ["VC", "Investor"] |

### Información de Firma

| Campo | Tipo | Ejemplo |
|-------|------|---------|
| `firm.name` | String | "Endeavor Catalyst" |
| `firm.slug` | String | "endeavor-catalyst" |
| `firm.current_fund_size` | String | "$292M" |
| `position` | String | "managing_partner" |
| `headline` | String | "Managing Partner at Endeavor Catalyst" |

### Datos de Inversión

| Campo | Tipo | Ejemplo |
|-------|------|---------|
| `min_investment` | String | "100000" |
| `max_investment` | String | "5000000" |
| `target_investment` | String | "1500000" |
| `stages` | Array | [{ "display_name": "Seed" }, { "display_name": "Series A" }] |
| `investment_locations` | Array | [{ "display_name": "New York, New York" }] |
| `areas_of_interest` | Array | IDs de verticales |
| `leads_rounds` | Boolean | Indica si lidera rondas |

### Historial de Inversiones

| Campo | Tipo | Descripción |
|-------|------|-------------|
| `investments_on_record` | Connection | Lista de inversiones realizadas |
| `investor_profile_funding_rounds` | Connection | Rondas de funding participadas |

Cada inversión incluye:
- Nombre de la compañía
- Total levantado
- Co-inversores
- Etapa (Seed, Series A, B, etc.)
- Fecha
- Monto de la ronda
- Si fue lead investor
- Board role (si aplica)

### Red de Contactos

| Campo | Descripción |
|-------|-------------|
| `network_list_investor_profiles` | Co-inversores frecuentes |
| `network_list_scouts_and_angels_profiles` | Scouts y ángeles conectados |
| `investing_connections` | Conexiones de inversión |
| `first_degree_count` | Número de conexiones de primer grado |

### Educación y Experiencia

| Campo | Descripción |
|-------|-------------|
| `degrees` | Historial educativo (universidad, título, campo de estudio) |
| `positions` | Historial laboral con fechas |

---

## Evaluación para Kauffman Fellows

### Cobertura de Campos

| Campo Kauffman | Signal | Cobertura | Notas |
|----------------|--------|-----------|-------|
| Name | ✅ | Alta | `person.name`, `first_name`, `last_name` |
| LinkedIn URL | ✅ | Alta | `person.linkedin_url` |
| Job Title | ✅ | Alta | `headline`, `position` |
| Current Firm | ✅ | Alta | `firm.name` |
| Investing Thesis | ⚠️ | Media | `areas_of_interest` (IDs), `areas_of_interest_freeform` |
| Preferred Stages | ✅ | Alta | `stages[]` |
| Portfolio Companies | ✅ | Alta | `investments_on_record` |
| Co-investors | ✅ | Alta | `coinvestor_names`, `network_list_investor_profiles` |
| Fund Size | ✅ | Alta | `firm.current_fund_size` |
| Investment Range | ✅ | Alta | `min_investment`, `max_investment`, `target_investment` |
| Location | ✅ | Alta | `location`, `investment_locations` |

### Comparativa con Otras Fuentes

| Característica | Apollo | Crunchbase | Signal |
|----------------|--------|------------|--------|
| Personal Data | ✅ Alta | ⚠️ Media | ⚠️ Media |
| Investment Data | ❌ Baja | ✅ Alta | ✅ Alta |
| Precio | $$$  | $$$ | **Gratis** |
| API Oficial | ✅ Sí | ✅ Sí | ❌ No oficial |
| Rate Limits | Conocidos | Conocidos | Desconocidos |

---

## Limitaciones

1. **API No Oficial**: No hay documentación pública de la API GraphQL
2. **Rate Limits Desconocidos**: Puede haber límites no documentados
3. **Términos de Servicio**: Verificar si scraping/API está permitido
4. **Cobertura**: Principalmente enfocado en ecosistema VC de EE.UU.
5. **Datos Parciales**: Algunos perfiles pueden tener información incompleta

---

## Recursos

### Documentación
- [Qué es Signal](https://products.nfx.com/en/articles/4793136-what-is-signal)
- [¿Es Signal Gratis?](https://products.nfx.com/en/articles/4809043-is-signal-free)

### Librería No Oficial
- [GitHub: signal.nfx](https://github.com/ChrisMichaelPerezSantiago/signal.nfx) - API wrapper en JavaScript

---

## Archivos en esta Carpeta

| Archivo | Descripción |
|---------|-------------|
| `README.md` | Este documento |
| `examples/autocomplete-query.json` | Ejemplo de query de autocomplete |
| `examples/autocomplete-response.json` | Respuesta del autocomplete |
| `examples/investor-profile-query.json` | Query completa de perfil |
| `examples/investor-profile-response.json` | Respuesta completa de perfil |

---

*Última actualización: Diciembre 2024*


# Signal NFX

> **Documentación completa**: Ver [signal/README.md](signal/README.md)

## Resumen

[Signal](https://signal.nfx.com/) es una plataforma **gratuita** de NFX para búsqueda de inversores.

### ¿Sirve para Kauffman Fellows?

**Sí** ✅ - Signal proporciona datos de inversión que pueden complementar o reemplazar a Crunchbase:

| Dato | Disponibilidad |
|------|----------------|
| Perfil de inversor | ✅ |
| Historial de inversiones | ✅ |
| Etapas preferidas | ✅ |
| Rango de inversión | ✅ |
| Co-inversores | ✅ |
| Fund size | ✅ |
| URLs sociales | ✅ |

### Acceso

1. **Sin cuenta**: Perfiles públicos via `https://signal.nfx.com/investors/{nombre-apellido}`
2. **Con cuenta (gratis)**: API GraphQL en `https://signal-api.nfx.com/graphql`

### Archivos

```
signal/
├── README.md                              # Documentación completa
└── examples/
    ├── autocomplete-query.json            # Query de búsqueda
    ├── autocomplete-response.json         # Respuesta de búsqueda
    ├── investor-profile-query.json        # Query de perfil completo
    └── investor-profile-response.json     # Respuesta con datos de Allen Taylor
```


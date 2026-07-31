# Diagrama MER - Sivar Express (MercadoDigital SV)
```mermaid
---
title: Diagrama MER Lógico - MercadoDigital SV
---
erDiagram

    INTEGRANTES {
        string Fabricio_Abrego_Quijada
        string Gerardo_Chavez_Guillen
        string Ricardo_Ramirez_Escobar
        string Mario_Urquiza_Funes
    }

    DEPARTAMENTO {
        int id_departamento PK
        varchar_100 nombre UK
    }

    MUNICIPIO {
        int id_municipio PK
        varchar_100 nombre
        int id_departamento FK
    }

    COMERCIO {
        int id_comercio PK
        varchar_150 nombre
        varchar_17 nit UK
        varchar_100 correo
        varchar_20 telefono
        varchar_250 direccion
    }

    CLIENTE {
        int id_cliente PK
        varchar_20 tipo_cliente
        varchar_150 nombre
        varchar_10 dui
        varchar_17 nit
        varchar_20 nrc
        varchar_150 giro
        varchar_20 telefono
        varchar_100 correo
        varchar_250 direccion
        int id_municipio FK
    }

    PRODUCTO {
        varchar_10 id_producto PK
        int id_comercio FK
        varchar_120 nombre
        numeric_10_2 precio_actual
        varchar_250 descripcion
        boolean activo
    }

    BODEGA {
        int id_bodega PK
        varchar_100 nombre
        varchar_200 direccion
    }

    INVENTARIO_BODEGA {
        varchar_10 id_producto PK,FK
        int id_bodega PK,FK
        int stock_actual
        int stock_minimo
    }

    PEDIDO {
        bigint id_pedido PK
        timestamp fecha_hora
        int id_cliente FK
        numeric_10_2 costo_envio
        varchar_30 metodo_pago
        varchar_30 estado_actual
    }

    DETALLE_PEDIDO {
        bigint id_detalle PK
        bigint id_pedido FK
        varchar_10 id_producto FK
        int cantidad
        numeric_10_2 precio_unitario_historico
    }

    CUPON {
        int id_cupon PK
        varchar_30 codigo UK
        varchar_20 tipo_descuento
        numeric_10_2 valor
        date fecha_inicio
        date fecha_fin
        numeric_10_2 monto_minimo
        int limite_usos
    }

    PEDIDO_CUPON {
        bigint id_pedido PK,FK
        int id_cupon PK,FK
        numeric_10_2 monto_descontado
    }

    HISTORIAL_ESTADO {
        int id_historial PK
        bigint id_pedido FK
        varchar_30 estado
        timestamp fecha_hora
        varchar_100 usuario_responsable
    }

    DTE {
        bigint id_dte PK
        bigint id_pedido FK,UK
        varchar_2 tipo_dte
        uuid codigo_generacion UK
        char_40 sello_recepcion
        varchar_50 numero_control UK
        timestamp fecha_emision
        numeric_10_2 subtotal
        numeric_10_2 descuento
        numeric_10_2 iva
        numeric_10_2 retencion
        numeric_10_2 total_pagar
        varchar_20 estado_mh
    }

    %% ===== Relaciones =====

    DEPARTAMENTO ||--o{ MUNICIPIO         : "contiene"
    MUNICIPIO    ||--o{ CLIENTE           : "ubica"

    COMERCIO     ||--o{ PRODUCTO          : "vende"

    PRODUCTO     ||--o{ INVENTARIO_BODEGA : "se almacena en"
    BODEGA       ||--o{ INVENTARIO_BODEGA : "contiene"

    CLIENTE      ||--o{ PEDIDO            : "realiza"
    PEDIDO       ||--|{ DETALLE_PEDIDO    : "incluye"
    PRODUCTO     ||--o{ DETALLE_PEDIDO    : "es vendido en"

    PEDIDO       ||--o{ PEDIDO_CUPON      : "aplica"
    CUPON        ||--o{ PEDIDO_CUPON      : "es usado en"

    PEDIDO       ||--o{ HISTORIAL_ESTADO  : "registra cambios en"

    PEDIDO       ||--|| DTE               : "genera"

    
# 📚 API Externa para Empresas - Documentación Completa

Esta API permite a empresas externas integrar con el sistema Coco Market usando autenticación dual por API Key + Secret Key.

## 🔐 Modelo de Seguridad

**IMPORTANTE**: Esta API usa autenticación dual con dos keys obligatorias:

- **🔑 API Key**: Identifica tu empresa (pública relativa, puede estar en logs)
- **🔐 Secret Key**: Autentica tus requests (privada, nunca debe exponerse)

**Ambas keys son requeridas en todos los requests.** Este modelo garantiza máxima seguridad al requerir tanto identificación como autenticación.

## � Proceso de Integración para Empresas

### ¿Por qué necesito configurar mi empresa en el sistema?

Coco Market requiere que las empresas externas se registren por las siguientes razones:

1. **🔐 Seguridad**: Cada empresa tiene API Keys únicas para autenticación segura
2. **📊 Tracking**: Todas las órdenes quedan asociadas a tu empresa para seguimiento
3. **🔔 Webhooks Personalizados**: Recibir notificaciones en tu propio sistema
4. **📈 Rate Limiting**: Límites de uso personalizados según tu plan
5. **💰 Facturación**: Asociar transacciones y comisiones a tu empresa
6. **🛡️ Aislamiento**: Cada empresa solo puede ver sus propias órdenes

### ¿Cómo me registro como empresa externa?

**Contacta al equipo de Coco Market con la siguiente información:**

```
Empresa: [Nombre de tu empresa]
Email de contacto: [email@tuempresa.com]
Webhook URL: [https://tu-api.com/webhooks] (opcional inicialmente)
Volumen estimado: [órdenes por mes aproximadamente]
```

**El equipo de Coco Market te proporcionará:**

1. ✅ **API Key**: Tu clave pública de autenticación
2. ✅ **Secret Key**: Tu clave secreta de autenticación (requerida junto con API Key)
3. ✅ **Empresa ID**: Tu identificador interno en el sistema
4. ✅ **Webhook Secret**: Clave para verificar la autenticidad de los webhooks
5. ✅ **Rate Limits**: Límites personalizados según tu plan
6. ✅ **Acceso a documentación**: Esta documentación completa

### ¿Qué información necesito configurar?

#### 🔑 **API Keys** (Proporcionadas por Coco Market)

- **API Key**: Clave pública para identificar tu empresa
- **Secret Key**: Clave secreta para autenticar tus requests
- **Formato API Key**: `coco_[40_caracteres_aleatorios]`
- **Formato Secret Key**: `[43_caracteres_aleatorios]`
- **Uso**: Headers `X-API-Key` y `X-Secret-Key` en cada petición
- **Seguridad**: Nunca expongas estas keys en código frontend, especialmente la Secret Key

**🚨 SEGURIDAD CRÍTICA - Secret Key:**

- NUNCA incluir en URLs, logs de aplicación, o código frontend
- NUNCA commitear en repositorios públicos
- Almacenar solo en variables de entorno o sistemas seguros de secrets
- Regenerar inmediatamente si se compromete

#### 🔔 **Webhook Configuration** (Tu responsabilidad)

```json
{
  "webhook_url": "https://tu-api.com/cocomarket-webhooks",
  "webhook_secret": "proporcionado_por_coco_market",
  "webhook_enabled": true,
  "webhook_events": ["order_status_changed", "payment_status_changed"]
}
```

**¿Por qué configurar webhooks?**

- **⚡ Tiempo Real**: Recibe notificaciones instantáneas de cambios
- **🔄 Automatización**: Actualiza tu sistema automáticamente
- **📱 Notificaciones**: Informa a tus clientes del progreso
- **🎯 Eficiencia**: Evita consultar constantemente el API
- **🛡️ Confiabilidad**: Sistema de reintentos garantiza entrega

**Ejemplo de flujo sin webhooks (ineficiente):**

```
Tu Sistema → API Poll cada 5min → ¿Cambió el estado? → Tal vez
```

**Ejemplo de flujo con webhooks (eficiente):**

```
Orden cambia estado → Coco Market → Webhook inmediato → Tu Sistema ✅
```

### 🛠️ Script de Configuración Automática

Si eres administrador de Coco Market, puedes usar el script de gestión:

```bash
# Crear nueva empresa con configuración completa
python manage_external_companies.py
```

**El script permite:**

1. 🏢 Crear nuevas empresas externas
2. 🔑 Generar API Keys adicionales
3. 🔔 Configurar webhooks
4. 📊 Ver empresas existentes
5. ⚙️ Actualizar configuraciones

## �🔐 Autenticación

Todas las peticiones requieren los siguientes headers:

```
X-API-Key: tu_api_key_aquí
X-Secret-Key: tu_secret_key_aquí
```

**⚠️ IMPORTANTE**: Ambas keys son obligatorias para todas las peticiones. La API rechazará requests que no incluyan ambos headers.

## 📋 Endpoints Disponibles

### 1. **POST** `/v1/external-api/register/`

Registra un nuevo usuario en el sistema.

**Payload:**

```json
{
  "email": "user@example.com",
  "password": "password123",
  "first_name": "Juan",
  "last_name": "Pérez",
  "phone": "+584121234567"
}
```

**Respuesta exitosa (201):**

```json
{
  "user_id": 123,
  "email": "user@example.com",
  "first_name": "Juan",
  "last_name": "Pérez",
  "status": "registered"
}
```

### 2. **POST** `/v1/external-api/login/`

Inicia sesión con credenciales de usuario.

**Payload:**

```json
{
  "email": "user@example.com",
  "password": "password123"
}
```

**Respuesta exitosa (200):**

```json
{
  "user_id": 123,
  "email": "user@example.com",
  "first_name": "Juan",
  "last_name": "Pérez",
  "user_type": "remitter",
  "phone": "+584121234567",
  "status": "enabled",
  "login_status": "success"
}
```

### 3. **GET** `/v1/external-api/whoami/?user_id=123`

Obtiene el perfil completo del usuario (idéntico al endpoint normal).

**Parámetros:**

- `user_id`: ID del usuario

**Respuesta exitosa (200):**

```json
{
  "whoami": {
    "first_name": "Jesus",
    "last_name": "Galvis",
    "email": "jesus.galvis@cocomercado.com",
    "password": null,
    "type": "remitter",
    "image": null,
    "name": "",
    "phone_numbers": [
      {
        "id": 343097,
        "phone_number": "+58 424-2672280",
        "phone_verified_at": "2023-11-02T12:53:49.709514-04:00",
        "phone_type": "mobile",
        "user_account": 339749,
        "country": 143
      }
    ],
    "referral_token": "Jesus3657",
    "general_credit": "0.00",
    "referral_credit": "0.00",
    "user_account_id": "339749",
    "identification_type": "CV",
    "identification": "25987389",
    "sex": null,
    "birth_date": null,
    "country": "",
    "currency": "",
    "is_term_condition": false
  },
  "settings": {
    "default_shipping_cost": 3.0,
    "free_shipping_threshold": 50.0,
    "external_shipping_cost": 5.0,
    "header_banner_end_at": 0,
    "header_banner_text": ""
  }
}
```

### 4. **GET** `/v1/external-api/sector/?search=valencia`

Busca sectores por nombre.

**Parámetros:**

- `search`: Término de búsqueda (mínimo 2 caracteres)

**Respuesta exitosa (200):**

```json
{
  "sectors": [
    {
      "id": 11959,
      "name": "Guaparo ( Valencia)",
      "city_name": "Valencia"
    }
  ],
  "count": 1
}
```

### 5. **POST** `/v1/external-api/cart/sector/`

Selecciona un sector para el carrito del usuario.

**Payload:**

```json
{
  "user_id": 123,
  "sector_id": 11959
}
```

**Respuesta exitosa (200):**

```json
{
  "message": "Sector selected successfully",
  "cart_id": 456,
  "sector_id": 11959,
  "sector_name": "Guaparo ( Valencia)"
}
```

### 6. **GET** `/v1/external-api/products/?sector_id=11959&text=combo`

Obtiene lista de productos filtrados.

**Parámetros:**

- `sector_id`: ID del sector (requerido)
- `text`: Texto de búsqueda (opcional)

**Respuesta exitosa (200):**

```json
{
  "products": [
    {
      "id": 789,
      "name": "Combo Familiar Premium",
      "description": "Delicioso combo para toda la familia",
      "price": "25.99",
      "discounted_price": "23.39",
      "images": ["https://example.com/image1.png"],
      "is_active": true,
      "category": "Combos",
      "store_name": "SuperMercado Central",
      "stock_quantity": 50
    }
  ],
  "count": 1,
  "sector_id": "11959"
}
```

### 7. **POST** `/v1/external-api/cart/add/`

Agrega un producto al carrito (funcionalidad idéntica a la API normal).

**Payload:**

```json
{
  "user_id": 123,
  "product_id": 789,
  "quantity": 2
}
```

**Respuesta exitosa (200):** Respuesta idéntica al endpoint normal del carrito con todos los detalles.

### 8. **GET** `/v1/external-api/cart/?user_id=123`

Obtiene el carrito actual del usuario con todos los productos.

**Parámetros:**

- `user_id`: ID del usuario

**Respuesta exitosa (200):**

```json
{
  "cart_id": 456,
  "sector": {
    "id": 11959,
    "name": "Guaparo ( Valencia)",
    "city_name": "Valencia"
  },
  "products": [
    {
      "id": 1,
      "product": {
        "id": 789,
        "name": "Combo Familiar Premium",
        "price": "25.99",
        "discounted_price": "23.39",
        "image": "https://example.com/image1.png",
        "store_name": "SuperMercado Central"
      },
      "quantity": 2,
      "total_price": "46.78"
    }
  ],
  "total_items": 2,
  "total_amount": "46.78",
  "shipping_cost": "3.00",
  "grand_total": "49.78"
}
```

### 9. **DELETE** `/v1/external-api/cart/?user_id=123`

Elimina productos del carrito del usuario.

**Parámetros:**

- `user_id`: ID del usuario (requerido)
- `store_id`: ID de la tienda (opcional, elimina solo productos de esa tienda)

**Respuesta exitosa (200):**

```json
{
  "message": "Cart cleared successfully",
  "removed_items": 2
}
```

### 10. **DELETE** `/v1/external-api/cart/product/{product_id}/?user_id=123`

Elimina un producto específico del carrito.

**Parámetros:**

- `product_id`: ID del producto a eliminar (en la URL)
- `user_id`: ID del usuario (en query params)

**Respuesta exitosa (200):**

```json
{
  "message": "Product removed from cart",
  "product_id": 789,
  "removed": true
}
```

### 11. **GET** `/v1/external-api/beneficiaries/?user_id=123&sector_id=11959`

Obtiene beneficiarios del usuario en un sector específico.

**Parámetros:**

- `user_id`: ID del usuario
- `sector_id`: ID del sector

**Respuesta exitosa (200):**

```json
{
  "beneficiaries": [
    {
      "beneficiary_id": 26165,
      "first_name": "María",
      "last_name": "González",
      "email": "",
      "phone_number": "+584127654321",
      "mobile_number": "",
      "address_line1": "Av. Principal #123",
      "city": null,
      "country": "VE",
      "sector": 11959,
      "created_at": "2025-08-28T11:01:04.793612-04:00"
    }
  ],
  "count": 1,
  "has_beneficiary_in_sector": true
}
```

### 12. **POST** `/v1/external-api/beneficiaries/`

Crea un nuevo beneficiario. **IMPORTANTE:** Este endpoint acepta exactamente los mismos campos que la API normal para garantizar que el beneficiario tenga toda la información necesaria para el delivery.

**Payload (campos disponibles):**

```json
{
  "user_id": 123,
  "first_name": "Pedro",
  "last_name": "Martínez",
  "email": "pedro.martinez@email.com",
  "phone_number": "+584129876543",
  "mobile_number": "+584121234567",
  "mobile_number2": "+584141234567",
  "address_line1": "Calle Secundaria #456",
  "address_line2": "Apartamento 3B",
  "city": "Caracas",
  "sector": 11959,
  "state": "Distrito Capital",
  "country": "VE",
  "zip_code": "1050",
  "residency": "Torre Residencial",
  "note": "Portón azul, llamar al timbre 3B"
}
```

**Campos mínimos requeridos:**

- `user_id`: ID del usuario/remitente
- `first_name`: Nombre del beneficiario
- `last_name`: Apellido del beneficiario
- `sector`: ID del sector para delivery
- `address_line1`: Dirección principal

**Campos recomendados para delivery:**

- `phone_number`: Teléfono principal de contacto
- `mobile_number`: Teléfono móvil adicional
- `email`: Email para notificaciones
- `address_line2`: Complemento de dirección
- `city`, `state`: Ubicación específica
- `note`: Instrucciones especiales para el delivery

**Respuesta exitosa (201):**

```json
{
  "message": "Beneficiary created successfully",
  "beneficiary": {
    "beneficiary_id": 26166,
    "first_name": "Pedro",
    "last_name": "Martínez",
    "email": "pedro.martinez@email.com",
    "phone_number": "+584129876543",
    "is_phone_number_valid": null,
    "mobile_number": "+584121234567",
    "mobile_number2": "+584141234567",
    "address_line1": "Calle Secundaria #456",
    "address_line2": "Apartamento 3B",
    "city": "Caracas",
    "sector": 11959,
    "state": "Distrito Capital",
    "country": "VE",
    "zip_code": "1050",
    "residency": "Torre Residencial",
    "note": "Portón azul, llamar al timbre 3B",
    "remitter": 123,
    "created_at": "2025-08-28T17:30:00.000000-04:00",
    "modified_at": "2025-08-28T17:30:00.000000-04:00",
    "is_deleted": false
  }
}
```

### 13. **GET** `/v1/external-api/company/orders/`

Obtiene las órdenes de la empresa externa con filtros y paginación.

**Parámetros de filtro (opcionales):**

- `order_id`: ID específico de orden
- `status`: Estado de la orden (`pending`, `in_progress`, `shipped`, `delivered`, `cancelled`)
- `payment_status`: Estado de pago (`pending`, `paid`, `refunded`, `partially_refunded`)
- `created_from`: Fecha desde (formato: YYYY-MM-DD)
- `created_to`: Fecha hasta (formato: YYYY-MM-DD)
- `page`: Número de página para paginación
- `page_size`: Cantidad de resultados por página (máximo 100)

**Respuesta exitosa (200):**

```json
{
  "count": 25,
  "next": "http://localhost:8000/v1/external-api/company/orders/?page=2",
  "previous": null,
  "results": [
    {
      "id": 512427,
      "order_number": "VLC001-512427",
      "status": "delivered",
      "payment_status": "paid",
      "order_total": "49.78",
      "order_subtotal": "46.78",
      "order_tax": "0.00",
      "order_shipping": "3.00",
      "created": "2025-08-28T17:08:04.099450-04:00",
      "modified": "2025-08-28T18:30:15.234567-04:00",
      "delivered_at": "2025-08-28T19:45:00.000000-04:00",
      "remitter": {
        "id": 123,
        "identification": "25987389",
        "user": {
          "first_name": "Juan",
          "last_name": "Pérez",
          "email": "user@example.com"
        }
      },
      "beneficiary": {
        "beneficiary_id": "26165",
        "first_name": "María",
        "last_name": "González",
        "phone_number": "+584127654321",
        "address_line1": "Av. Principal #123",
        "city": "Valencia",
        "sector": {
          "id": 11959,
          "name": "Guaparo ( Valencia)"
        }
      },
      "products": [
        {
          "id": 789,
          "name": "Combo Familiar Premium",
          "price": "23.39",
          "quantity": 2,
          "total": "46.78",
          "store_name": "SuperMercado Central"
        }
      ],
      "transaction": {
        "id": 456648,
        "payment_type": "cash",
        "charged": "49.78",
        "created_at": "2025-08-28T17:08:04.074208-04:00"
      }
    }
  ]
}
```

### 14. **POST** `/v1/external-api/orders/`

Crea una nueva orden (funcionalidad idéntica a la API normal).

**Payload:**

```json
{
  "user_id": 123,
  "beneficiary_id": 26165,
  "payment_gateway": "cash"
}
```

**Respuesta exitosa (201):** Estructura completa idéntica a la API normal:

```json
{
  "transaction": {
    "id": 456648,
    "beneficiary": {
      "sector": {
        "name": "Guaparo ( Valencia)",
        "city": {
          "name": "Valencia",
          "code": "VLC",
          "state": "Carabobo"
        }
      },
      "first_name": "María",
      "last_name": "González",
      "email": "",
      "phone_number": "+584127654321",
      "address_line1": "Av. Principal #123",
      "country": "VE",
      "created_at": "2025-08-28T11:01:04.793612-04:00"
    },
    "charged": 49.78,
    "cococredits": 0.0,
    "coupon_discount": 0.0,
    "coupon": null,
    "created_at": "2025-08-28T21:08:04.074208+00:00",
    "discount": 0,
    "orders": [
      {
        "id": 512410,
        "order_total": 49.78,
        "products": [
          {
            "id": 789,
            "name": "Combo Familiar Premium",
            "price": 23.39,
            "quantity": 2,
            "tax": 0.0,
            "images": ["https://example.com/image1.png"],
            "categories": ["Combos"]
          }
        ],
        "beneficiary": {
          "beneficiary_id": 26165,
          "first_name": "María",
          "last_name": "González"
        },
        "order_number": "VLC001-512410",
        "order_subtotal": "46.78",
        "order_tax": "0.00",
        "order_shipping": "3.00",
        "status": "pending",
        "payment_status": "pending",
        "payment_type": "cash",
        "created": "2025-08-28T17:08:04.099450-04:00"
      }
    ],
    "payment_method": "Efectivo",
    "payment_status": "pending",
    "payment_type": "cash",
    "remitter": {
      "user": {
        "first_name": "Juan",
        "last_name": "Pérez",
        "email": "user@example.com"
      }
    },
    "shipping": 3.0,
    "subtotal": 46.78,
    "tax": 0.0,
    "total": 49.78
  },
  "errors": "[]"
}
```

## 🔧 Códigos de Error

- **400**: Parámetros inválidos o faltantes
- **401**: API Key inválida o faltante
- **404**: Recurso no encontrado (usuario, producto, beneficiario, etc.)
- **500**: Error interno del servidor

## 📝 Ejemplo de Implementación Completa (Python)

```python
import requests
import json

API_BASE_URL = "http://localhost:8000/v1/external-api"
API_KEY = "tu_api_key_aquí"
SECRET_KEY = "tu_secret_key_aquí"

headers = {
    "X-API-Key": API_KEY,
    "X-Secret-Key": SECRET_KEY,
    "Content-Type": "application/json"
}

class CocoMarketAPI:
    def __init__(self, api_key, secret_key, base_url=API_BASE_URL):
        self.base_url = base_url
        self.headers = {
            "X-API-Key": api_key,
            "X-Secret-Key": secret_key,
            "Content-Type": "application/json"
        }

    # Autenticación y usuarios
    def register_user(self, email, password, first_name, last_name, phone):
        """Registra un nuevo usuario"""
        url = f"{self.base_url}/register/"
        data = {
            "email": email,
            "password": password,
            "first_name": first_name,
            "last_name": last_name,
            "phone": phone
        }
        response = requests.post(url, json=data, headers=self.headers)
        return response.json()

    def login_user(self, email, password):
        """Inicia sesión con credenciales"""
        url = f"{self.base_url}/login/"
        data = {"email": email, "password": password}
        response = requests.post(url, json=data, headers=self.headers)
        return response.json()

    def get_user_profile(self, user_id):
        """Obtiene perfil completo del usuario"""
        url = f"{self.base_url}/whoami/"
        params = {"user_id": user_id}
        response = requests.get(url, params=params, headers=self.headers)
        return response.json()

    # Sectores y ubicación
    def search_sectors(self, search_term):
        """Busca sectores por nombre"""
        url = f"{self.base_url}/sector/"
        params = {"search": search_term}
        response = requests.get(url, params=params, headers=self.headers)
        return response.json()

    def select_sector(self, user_id, sector_id):
        """Selecciona un sector para el carrito"""
        url = f"{self.base_url}/cart/sector/"
        data = {"user_id": user_id, "sector_id": sector_id}
        response = requests.post(url, json=data, headers=self.headers)
        return response.json()

    # Productos
    def search_products(self, sector_id, search_text=None):
        """Busca productos en un sector"""
        url = f"{self.base_url}/products/"
        params = {"sector_id": sector_id}
        if search_text:
            params["text"] = search_text
        response = requests.get(url, params=params, headers=self.headers)
        return response.json()

    # Carrito
    def get_cart(self, user_id):
        """Obtiene el carrito del usuario"""
        url = f"{self.base_url}/cart/"
        params = {"user_id": user_id}
        response = requests.get(url, params=params, headers=self.headers)
        return response.json()

    def add_to_cart(self, user_id, product_id, quantity):
        """Agrega producto al carrito"""
        url = f"{self.base_url}/cart/add/"
        data = {
            "user_id": user_id,
            "product_id": product_id,
            "quantity": quantity
        }
        response = requests.post(url, json=data, headers=self.headers)
        return response.json()

    def clear_cart(self, user_id, store_id=None):
        """Elimina productos del carrito"""
        url = f"{self.base_url}/cart/"
        params = {"user_id": user_id}
        if store_id:
            params["store_id"] = store_id
        response = requests.delete(url, params=params, headers=self.headers)
        return response.json()

    def remove_product_from_cart(self, user_id, product_id):
        """Elimina un producto específico del carrito"""
        url = f"{self.base_url}/cart/product/{product_id}/"
        params = {"user_id": user_id}
        response = requests.delete(url, params=params, headers=self.headers)
        return response.json()

    # Beneficiarios
    def get_beneficiaries(self, user_id, sector_id):
        """Obtiene beneficiarios del usuario en un sector"""
        url = f"{self.base_url}/beneficiaries/"
        params = {"user_id": user_id, "sector_id": sector_id}
        response = requests.get(url, params=params, headers=self.headers)
        return response.json()

    def create_beneficiary(self, user_id, first_name, last_name, phone_number, address, sector, country="VE"):
        """Crea un nuevo beneficiario"""
        url = f"{self.base_url}/beneficiaries/"
        data = {
            "user_id": user_id,
            "first_name": first_name,
            "last_name": last_name,
            "phone_number": phone_number,
            "address_line1": address,
            "sector": sector,
            "country": country
        }
        response = requests.post(url, json=data, headers=self.headers)
        return response.json()

    # Órdenes
    def create_order(self, user_id, beneficiary_id, payment_gateway="cash"):
        """Crea una nueva orden"""
        url = f"{self.base_url}/orders/"
        data = {
            "user_id": user_id,
            "beneficiary_id": beneficiary_id,
            "payment_gateway": payment_gateway
        }
        response = requests.post(url, json=data, headers=self.headers)
        return response.json()

# Ejemplo de uso completo
def main():
    api = CocoMarketAPI("tu_api_key_aquí", "tu_secret_key_aquí")

    # 1. Registrar usuario
    user_data = api.register_user(
        email="test@example.com",
        password="password123",
        first_name="Juan",
        last_name="Pérez",
        phone="+584121234567"
    )
    print(f"Usuario registrado: {user_data}")

    user_id = user_data["user_id"]

    # 2. Buscar sectores
    sectors = api.search_sectors("valencia")
    sector_id = sectors["sectors"][0]["id"]
    print(f"Sector encontrado: {sector_id}")

    # 3. Seleccionar sector
    api.select_sector(user_id, sector_id)

    # 4. Buscar productos
    products = api.search_products(sector_id, "combo")
    product_id = products["products"][0]["id"]
    print(f"Producto encontrado: {product_id}")

    # 5. Agregar al carrito
    api.add_to_cart(user_id, product_id, 2)

    # 6. Ver carrito
    cart = api.get_cart(user_id)
    print(f"Carrito: {json.dumps(cart, indent=2)}")

    # 7. Crear beneficiario
    beneficiary = api.create_beneficiary(
        user_id=user_id,
        first_name="María",
        last_name="González",
        phone_number="+584127654321",
        address="Av. Principal #123",
        sector=sector_id
    )
    beneficiary_id = beneficiary["beneficiary_id"]

    # 8. Crear orden
    order = api.create_order(user_id, beneficiary_id, "cash")
    print(f"Orden creada: {order['transaction']['id']}")

if __name__ == "__main__":
    main()
```

## 🚀 Flujo Típico de Integración

1. **Registrar/Login** del usuario
2. **Obtener perfil** completo del usuario (whoami)
3. **Buscar sectores** disponibles para delivery
4. **Seleccionar sector** para el carrito
5. **Buscar productos** en ese sector
6. **Agregar productos** al carrito
7. **Revisar carrito** antes de proceder
8. **Verificar/crear beneficiario** en el sector
9. **Crear orden** con el beneficiario seleccionado
10. **Procesar respuesta** de la transacción completa
11. **Monitorear estado de órdenes** vía webhooks o consulta periódica

## 🔔 Sistema de Webhooks

### Configuración de Webhooks

Los webhooks permiten a tu aplicación recibir notificaciones automáticas cuando ocurren eventos importantes en las órdenes de tu empresa. Debes configurar tu empresa externa con:

- **webhook_url**: URL endpoint donde recibirás las notificaciones
- **webhook_secret**: Clave secreta para verificar la autenticidad de los webhooks
- **webhook_enabled**: `true` para activar las notificaciones
- **webhook_events**: Array de eventos que deseas recibir

### Eventos Disponibles

#### 1. `order_status_changed`

Se dispara cuando cambia el estado de una orden.

**Estados posibles:**

- `pending` → `in_progress`: Orden confirmada y en preparación
- `in_progress` → `shipped`: Orden enviada para delivery
- `shipped` → `delivered`: Orden entregada exitosamente
- `*` → `cancelled`: Orden cancelada

#### 2. `payment_status_changed`

Se dispara cuando cambia el estado de pago de una orden.

**Estados de pago:**

- `pending` → `paid`: Pago procesado exitosamente
- `paid` → `refunded`: Reembolso completo procesado
- `paid` → `partially_refunded`: Reembolso parcial procesado

#### 3. `order_created`

Se dispara cuando se crea una nueva orden (opcional, ya que la empresa es quien la crea).

#### 4. `order_updated`

Se dispara cuando se actualiza información general de la orden.

### Estructura del Webhook

**Headers HTTP enviados:**

```http
POST /tu-webhook-endpoint HTTP/1.1
Content-Type: application/json
X-Webhook-Signature: sha256=a1b2c3d4e5f6...
X-Webhook-Event: order_status_changed
User-Agent: CocoMarket-Webhook/1.0
```

**Payload del webhook:**

```json
{
  "event_type": "order_status_changed",
  "timestamp": "2025-09-01T15:30:45.123456Z",
  "external_company_id": 4,
  "external_company_name": "Tu Empresa",
  "changes": {
    "field": "status",
    "old_value": "in_progress",
    "new_value": "shipped"
  },
  "order": {
    "id": 512427,
    "order_number": "VLC001-512427",
    "status": "shipped",
    "payment_status": "paid",
    "order_total": 49.78,
    "order_subtotal": 46.78,
    "order_tax": 0.0,
    "order_shipping": 3.0,
    "created": "2025-08-28T17:08:04.099450-04:00",
    "modified": "2025-09-01T15:30:45.123456-04:00",
    "delivered_at": null,
    "remitter": {
      "id": 123,
      "identification": "25987389",
      "user_id": 339749
    },
    "beneficiary": {
      "beneficiary_id": "26165",
      "first_name": "María",
      "last_name": "González",
      "city": "Valencia"
    }
  }
}
```

### Verificación de Firma (HMAC)

Para garantizar la autenticidad de los webhooks, cada payload incluye una firma HMAC-SHA256 en el header `X-Webhook-Signature`.

**Verificación en Python:**

```python
import hmac
import hashlib
import json

def verify_webhook(payload_body, signature, secret):
    """
    Verifica la firma HMAC del webhook

    Args:
        payload_body: El cuerpo JSON del webhook como string
        signature: El valor del header X-Webhook-Signature
        secret: Tu webhook_secret configurado

    Returns:
        bool: True si la firma es válida
    """
    # Remover el prefijo 'sha256=' si existe
    if signature.startswith('sha256='):
        signature = signature[7:]

    # Calcular la firma esperada
    expected_signature = hmac.new(
        secret.encode('utf-8'),
        payload_body.encode('utf-8'),
        hashlib.sha256
    ).hexdigest()

    # Comparación segura para evitar timing attacks
    return hmac.compare_digest(signature, expected_signature)

# Ejemplo de uso en Flask
from flask import Flask, request, jsonify

app = Flask(__name__)

@app.route('/webhook', methods=['POST'])
def handle_webhook():
    payload_body = request.get_data(as_text=True)
    signature = request.headers.get('X-Webhook-Signature', '')
    event_type = request.headers.get('X-Webhook-Event', '')

    # Tu secret configurado en la empresa externa
    webhook_secret = "tu-secret-aqui"

    # Verificar firma
    if not verify_webhook(payload_body, signature, webhook_secret):
        return jsonify({"error": "Invalid signature"}), 401

    # Parsear el payload
    try:
        webhook_data = json.loads(payload_body)
    except json.JSONDecodeError:
        return jsonify({"error": "Invalid JSON"}), 400

    # Procesar según el tipo de evento
    if event_type == 'order_status_changed':
        handle_order_status_change(webhook_data)
    elif event_type == 'payment_status_changed':
        handle_payment_status_change(webhook_data)

    # Responder con 200 para confirmar recepción
    return jsonify({"status": "received"}), 200

def handle_order_status_change(data):
    """Procesa cambios de estado de orden"""
    order_id = data['order']['id']
    old_status = data['changes']['old_value']
    new_status = data['changes']['new_value']

    print(f"Orden {order_id}: {old_status} → {new_status}")

    # Tu lógica aquí: actualizar base de datos, enviar notificaciones, etc.
    if new_status == 'delivered':
        send_delivery_confirmation(data['order'])
    elif new_status == 'cancelled':
        process_cancellation(data['order'])

def handle_payment_status_change(data):
    """Procesa cambios de estado de pago"""
    order_id = data['order']['id']
    old_status = data['changes']['old_value']
    new_status = data['changes']['new_value']

    print(f"Pago orden {order_id}: {old_status} → {new_status}")

    if new_status == 'paid':
        confirm_payment(data['order'])
    elif new_status == 'refunded':
        process_refund(data['order'])
```

**Verificación en Node.js:**

```javascript
const crypto = require('crypto');
const express = require('express');

const app = express();

// Middleware para obtener el raw body
app.use('/webhook', express.raw({ type: 'application/json' }));

function verifyWebhook(payload, signature, secret) {
  // Remover el prefijo 'sha256=' si existe
  if (signature.startsWith('sha256=')) {
    signature = signature.substring(7);
  }

  // Calcular la firma esperada
  const expectedSignature = crypto
    .createHmac('sha256', secret)
    .update(payload, 'utf8')
    .digest('hex');

  // Comparación segura
  return crypto.timingSafeEqual(
    Buffer.from(signature, 'hex'),
    Buffer.from(expectedSignature, 'hex')
  );
}

app.post('/webhook', (req, res) => {
  const payload = req.body.toString('utf8');
  const signature = req.headers['x-webhook-signature'] || '';
  const eventType = req.headers['x-webhook-event'] || '';

  const webhookSecret = 'tu-secret-aqui';

  // Verificar firma
  if (!verifyWebhook(payload, signature, webhookSecret)) {
    return res.status(401).json({ error: 'Invalid signature' });
  }

  // Parsear el payload
  let webhookData;
  try {
    webhookData = JSON.parse(payload);
  } catch (error) {
    return res.status(400).json({ error: 'Invalid JSON' });
  }

  // Procesar según el tipo de evento
  switch (eventType) {
    case 'order_status_changed':
      handleOrderStatusChange(webhookData);
      break;
    case 'payment_status_changed':
      handlePaymentStatusChange(webhookData);
      break;
    default:
      console.log(`Evento desconocido: ${eventType}`);
  }

  // Confirmar recepción
  res.status(200).json({ status: 'received' });
});

function handleOrderStatusChange(data) {
  const orderId = data.order.id;
  const oldStatus = data.changes.old_value;
  const newStatus = data.changes.new_value;

  console.log(`Orden ${orderId}: ${oldStatus} → ${newStatus}`);

  // Tu lógica aquí
  if (newStatus === 'delivered') {
    sendDeliveryConfirmation(data.order);
  }
}

app.listen(3000, () => {
  console.log('Webhook server listening on port 3000');
});
```

### Configuración de tu Endpoint

Tu endpoint de webhook debe:

1. **Responder rápidamente**: Máximo 30 segundos
2. **Retornar HTTP 200**: Para confirmar recepción exitosa
3. **Ser idempotente**: El mismo evento puede llegar múltiples veces
4. **Verificar la firma**: Siempre validar el HMAC
5. **Manejar reintentos**: Si fallas, reintentaremos hasta 3 veces

### Reintentos y Manejo de Errores

- **Reintentos automáticos**: 3 intentos con backoff exponencial
- **Timeouts**: Si no respondes en 30 segundos, se considera fallido
- **Códigos de éxito**: 200-299 se consideran exitosos
- **Desactivación automática**: Después de múltiples fallos, se puede desactivar temporalmente

### Testing de Webhooks

Para probar tu implementación de webhooks:

1. **Usa webhook.site**: Para testing inicial sin código
2. **ngrok**: Para exponer tu desarrollo local
3. **Logs**: Revisa los logs de delivery en tu dashboard de empresa
4. **Simulador**: Usa el script `webhook_simulator.py` incluido en el sistema

**Ejemplo con webhook.site:**

1. Ve a https://webhook.site y copia tu URL única
2. Configura esa URL como tu webhook_url
3. Crea una orden de prueba
4. Cambia el estado de la orden
5. Observa el webhook en webhook.site

### Estructura de la Base de Datos de Webhooks

El sistema mantiene un registro completo de todos los webhooks enviados:

```sql
-- WebhookDelivery: registro de cada envío
{
  "id": 12345,
  "company_id": 4,
  "order_id": 512427,
  "event_type": "order_status_changed",
  "webhook_url": "https://tu-api.com/webhook",
  "status": "delivered", // pending, sent, delivered, failed
  "attempt_count": 1,
  "response_status_code": 200,
  "response_body": "{'status': 'received'}",
  "error_message": null,
  "signature": "sha256=a1b2c3d4...",
  "payload": { ... }, // El payload completo enviado
  "created_at": "2025-09-01T15:30:45Z",
  "delivered_at": "2025-09-01T15:30:46Z"
}
```

## 🛡️ Limitaciones y Consideraciones

### Autenticación

- **Dual Keys obligatorias**: Todas las peticiones deben incluir headers `X-API-Key` y `X-Secret-Key`
- **Validación estricta**: Ambas keys son verificadas en cada request
- **Secret Key segura**: La Secret Key nunca debe exponerse en logs o frontend

### Rate Limits

- **1000 requests por hora** por API Key por defecto
- **10 requests por minuto** para endpoints de creación (register, beneficiaries, orders)

### Timeouts y Tamaños

- **Timeout**: 30 segundos máximo por request
- **Tamaño máximo**: 10MB por payload
- **Formatos soportados**: JSON únicamente
- **Webhook timeout**: 30 segundos para respuesta de webhooks

### Funcionalidad

- **Carrito persistente**: Los productos permanecen en el carrito entre sesiones
- **Validaciones de sector**: Los productos solo se muestran si están disponibles en el sector seleccionado
- **Beneficiarios por sector**: Cada beneficiario está asociado a un sector específico
- **Órdenes completas**: La respuesta de creación de orden incluye toda la información detallada
- **Webhooks confiables**: Sistema de reintentos automáticos con backoff exponencial

### Consideraciones de Desarrollo

- **Manejo de errores**: Siempre verificar el status code y manejar errores apropiadamente
- **IDs de usuario**: Guardar el `user_id` obtenido durante login/registro para usar en subsecuentes llamadas
- **Verificación de carrito**: Revisar el carrito antes de crear órdenes para confirmar productos disponibles
- **Beneficiarios únicos**: Un beneficiario puede existir en múltiples sectores pero debe crearse separadamente para cada uno
- **Idempotencia de webhooks**: El mismo evento puede llegar múltiples veces, diseña tu lógica para ser idempotente
- **Verificación de firma**: Siempre verificar la firma HMAC de los webhooks antes de procesarlos
- **Monitoreo de órdenes**: Combinar webhooks con consultas periódicas al endpoint de órdenes para máxima confiabilidad

### Webhooks - Consideraciones Específicas

- **Endpoint público**: Tu webhook endpoint debe ser accesible públicamente por HTTPS
- **Respuesta rápida**: Procesar webhooks rápidamente, usar colas para trabajo pesado
- **Registro de eventos**: Mantener logs de todos los webhooks recibidos para auditoría
- **Manejo de duplicados**: Usar el ID del evento o timestamp para evitar procesar el mismo evento múltiples veces
- **Configuración de eventos**: Solo activar los eventos que realmente necesitas procesar

## 📊 Códigos de Estado HTTP Comunes

| Código | Descripción           | Cuándo ocurre                                   |
| ------ | --------------------- | ----------------------------------------------- |
| 200    | OK                    | Operación exitosa                               |
| 201    | Created               | Recurso creado exitosamente                     |
| 400    | Bad Request           | Parámetros faltantes o inválidos                |
| 401    | Unauthorized          | API Key faltante o inválida                     |
| 404    | Not Found             | Usuario, producto, o beneficiario no encontrado |
| 429    | Too Many Requests     | Rate limit excedido                             |
| 500    | Internal Server Error | Error del servidor                              |

### Códigos Específicos de Webhooks

| Código  | Descripción                   | Acción del Sistema                               |
| ------- | ----------------------------- | ------------------------------------------------ |
| 200-299 | Webhook recibido exitosamente | Marcar como entregado, no reenviar               |
| 400-499 | Error del cliente             | No reenviar, marcar como fallido permanentemente |
| 500-599 | Error del servidor            | Reenviar hasta 3 veces con backoff exponencial   |
| Timeout | Sin respuesta en 30s          | Reenviar hasta 3 veces                           |

## 🔍 Debugging y Troubleshooting

### Errores Comunes

1. **"API Key inválida"**: Verificar que el header `X-API-Key` esté presente y correcto
2. **"User not found"**: Verificar que el `user_id` sea válido y el usuario exista
3. **"No active cart found"**: Asegurarse de seleccionar un sector primero
4. **"Cart is empty"**: Agregar productos al carrito antes de crear órdenes
5. **"Beneficiary not found"**: Verificar que el `beneficiary_id` sea correcto y pertenezca al usuario
6. **"Invalid webhook signature"**: Verificar que el webhook_secret sea correcto y la verificación HMAC esté bien implementada

### Errores de Webhooks

1. **Webhook no se recibe**:

   - Verificar que la URL sea accesible públicamente por HTTPS
   - Comprobar que el endpoint responda a POST requests
   - Revisar logs del servidor para errores

2. **Firma inválida**:

   - Verificar que el webhook_secret esté configurado correctamente
   - Comprobar la implementación de verificación HMAC
   - Asegurarse de usar el payload raw (sin parsear) para la verificación

3. **Eventos duplicados**:

   - Implementar lógica idempotente usando timestamps o IDs únicos
   - Los reintentos pueden causar duplicados legítimos

4. **Webhooks perdidos**:
   - Complementar con consultas periódicas al endpoint `/company/orders/`
   - Implementar reconciliación de estados periódica

### Logs y Monitoreo

La API registra todas las operaciones importantes. Para debugging, revisar:

- Requests con códigos 4xx y 5xx
- Tiempos de respuesta anormalmente altos
- Patrones de uso inusuales
- Fallos de webhooks en los logs de WebhookDelivery
- Rate limiting y patrones de tráfico

### Testing

#### API Testing

```bash
# Test básico de autenticación
curl -H "X-API-Key: tu_api_key" \
     -H "X-Secret-Key: tu_secret_key" \
     http://localhost:8000/v1/external-api/whoami/?user_id=123

# Test de órdenes de la empresa
curl -H "X-API-Key: tu_api_key" \
     -H "X-Secret-Key: tu_secret_key" \
     "http://localhost:8000/v1/external-api/company/orders/?status=delivered&page_size=5"
```

#### Webhook Testing

```python
# Script para testing local de webhooks
import requests

def test_webhook_endpoint():
    webhook_url = "http://localhost:3000/webhook"

    # Payload de prueba
    test_payload = {
        "event_type": "order_status_changed",
        "timestamp": "2025-09-01T15:30:45.123456Z",
        "order": {
            "id": 123456,
            "status": "delivered",
            "order_number": "TEST-123456"
        },
        "changes": {
            "field": "status",
            "old_value": "shipped",
            "new_value": "delivered"
        }
    }

    # Headers de prueba
    headers = {
        "Content-Type": "application/json",
        "X-Webhook-Event": "order_status_changed",
        "X-Webhook-Signature": "sha256=test_signature"
    }

    response = requests.post(webhook_url, json=test_payload, headers=headers)
    print(f"Response: {response.status_code} - {response.text}")
```

### Soporte

Para soporte técnico o problemas de integración:

- **Email**: api-support@cocomercado.com
- **Documentar el error**: Request/response completos con headers
- **Incluir timestamp**: Y API Key utilizada (sin exponer la key completa)
- **Para webhooks**: Incluir logs de tu endpoint y el ID del WebhookDelivery si está disponible
- **Ambiente**: Especificar si es desarrollo, testing o producción

### Herramientas Útiles

- **Postman Collection**: Disponible en `/client/Coco API.postman_collection.json`
- **Webhook Simulator**: Script `webhook_simulator.py` para probar tu endpoint
- **Testing Scripts**: Scripts en `/client/` para testing rápido
- **webhook.site**: Para testing inicial sin implementar tu endpoint

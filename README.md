# 📚 API Externa para Empresas - Documentación Completa

Esta API permite a empresas externas integrar con el sistema Coco Market usando autenticación por API Key.

## 🔐 Autenticación

Todas las peticiones requieren el header:

```
X-API-Key: tu_api_key_aquí
```

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

### 13. **POST** `/v1/external-api/orders/`

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

headers = {
    "X-API-Key": API_KEY,
    "Content-Type": "application/json"
}

class CocoMarketAPI:
    def __init__(self, api_key, base_url=API_BASE_URL):
        self.base_url = base_url
        self.headers = {
            "X-API-Key": api_key,
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
    api = CocoMarketAPI("tu_api_key_aquí")

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

## 🛡️ Limitaciones y Consideraciones

### Autenticación

- **API Key obligatoria**: Todas las peticiones deben incluir el header `X-API-Key`
- **Validación HMAC**: Algunas operaciones sensibles requieren validación adicional

### Rate Limits

- **1000 requests por hora** por API Key por defecto
- **10 requests por minuto** para endpoints de creación (register, beneficiaries, orders)

### Timeouts y Tamaños

- **Timeout**: 30 segundos máximo por request
- **Tamaño máximo**: 10MB por payload
- **Formatos soportados**: JSON únicamente

### Funcionalidad

- **Carrito persistente**: Los productos permanecen en el carrito entre sesiones
- **Validaciones de sector**: Los productos solo se muestran si están disponibles en el sector seleccionado
- **Beneficiarios por sector**: Cada beneficiario está asociado a un sector específico
- **Órdenes completas**: La respuesta de creación de orden incluye toda la información detallada

### Consideraciones de Desarrollo

- **Manejo de errores**: Siempre verificar el status code y manejar errores apropiadamente
- **IDs de usuario**: Guardar el `user_id` obtenido durante login/registro para usar en subsecuentes llamadas
- **Verificación de carrito**: Revisar el carrito antes de crear órdenes para confirmar productos disponibles
- **Beneficiarios únicos**: Un beneficiario puede existir en múltiples sectores pero debe crearse separadamente para cada uno

## 📊 Códigos de Estado HTTP Comunes

| Código | Descripción           | Cuándo ocurre                                   |
| ------ | --------------------- | ----------------------------------------------- |
| 200    | OK                    | Operación exitosa                               |
| 201    | Created               | Recurso creado exitosamente                     |
| 400    | Bad Request           | Parámetros faltantes o inválidos                |
| 401    | Unauthorized          | API Key faltante o inválida                     |
| 404    | Not Found             | Usuario, producto, o beneficiario no encontrado |
| 500    | Internal Server Error | Error del servidor                              |

## 🔍 Debugging y Troubleshooting

### Errores Comunes

1. **"API Key inválida"**: Verificar que el header `X-API-Key` esté presente y correcto
2. **"User not found"**: Verificar que el `user_id` sea válido y el usuario exista
3. **"No active cart found"**: Asegurarse de seleccionar un sector primero
4. **"Cart is empty"**: Agregar productos al carrito antes de crear órdenes
5. **"Beneficiary not found"**: Verificar que el `beneficiary_id` sea correcto y pertenezca al usuario

### Logs y Monitoreo

La API registra todas las operaciones importantes. Para debugging, revisar:

- Requests con códigos 4xx y 5xx
- Tiempos de respuesta anormalmente altos
- Patrones de uso inusuales

### Soporte

Para soporte técnico o problemas de integración:

- Email: api-support@cocomercado.com
- Documentar el error con request/response completos
- Incluir timestamp y API Key utilizada (sin exponer la key completa)

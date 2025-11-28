# Integrantes:

Lilliana Urbe

# Sistema de Gestión de Tienda Virtual - Arquitectura Limpia

Voy a crear un sistema de gestión de tienda virtual modular, aplicando los principios de arquitectura limpia.

## 📋 Requerimientos del Sistema

### Requerimientos Funcionales

#### Gestión de Productos e Inventario
*   **Registrar producto**: El sistema debe permitir crear nuevos productos en el catálogo con nombre, slug, descripción, marca, género y precio (RF-030).
*   **Mostrar productos**: El sistema debe mostrar una lista completa de todos los productos registrados (RF-031).
*   **Agregar variante de color**: El sistema debe permitir añadir colores disponibles a un producto (RF-037).
*   **Agregar variante de talla**: El sistema debe permitir asignar tallas a un producto con su respectivo stock y precio (RF-040).
*   **Visualizar variantes**: El sistema debe mostrar las variantes de color y talla disponibles para cada producto (RF-038, RF-041).

#### Gestión de Usuarios y Accesos
*   **Registro de usuarios**: El sistema debe permitir a los visitantes registrarse creando una cuenta nueva con nombre, correo, teléfono y contraseña (RF-001).
*   **Inicio de sesión**: El sistema debe permitir a los usuarios registrados iniciar sesión con su correo electrónico y contraseña (RF-002).
*   **Ver perfil de usuario**: El sistema debe permitir a los usuarios ver su información personal (nombre, correo, teléfonos, emails) (RF-003).
*   **Control de acceso por roles**: El sistema debe distinguir entre usuarios clientes y administradores (RF-011).

#### Experiencia de Compra (Carrito)
*   **Agregar al carrito**: El sistema debe permitir al cliente poner productos en su carrito de compras, seleccionando color y talla (RF-053).
*   **Ver carrito**: El sistema debe mostrar qué productos tiene el cliente en su carrito con sus subtotales (RF-054).
*   **Eliminar del carrito**: El sistema debe permitir sacar productos del carrito (RF-056).

### Requerimientos de Información

#### Gestión de Productos e Inventario
*   **Productos**: ID, nombre, slug, descripción, marca, género, precio, categoría (RI-030).
*   **Variantes**: Color (ID, nombre, default), Talla (ID, nombre, precio, cantidad/stock, SKU) (RI-037, RI-040).

#### Gestión de Usuarios y Accesos
*   **Usuarios**: ID, nombre, correo electrónico, contraseña (hash), teléfono, rol (customer/admin) (RI-001).

#### Experiencia de Compra
*   **Carrito**: ID, usuario, ítems (producto, cantidad, precio, variante color, variante talla) (RI-054).

### Requerimientos de Negocio

#### Gestión de Usuarios y Accesos
*   **Roles**: Existen dos roles principales: 'customer' (cliente) y 'admin' (administrador) (RN-011).
*   **Unicidad**: El correo electrónico debe ser único por usuario (RN-001).
*   **Seguridad**: Las contraseñas deben almacenarse encriptadas (hash) (RNF-006).

#### Experiencia de Compra
*   **Stock**: No se puede agregar al carrito una cantidad mayor al stock disponible de la variante seleccionada (RN-053).

### Requerimientos No Funcionales
*   **Rendimiento**: La aplicación debe cargar sus páginas principales en menos de 4 segundos (RNF-001).
*   **Seguridad**: Las contraseñas deben estar encriptadas y no ser visibles ni siquiera para administradores (RNF-006).
*   **Seguridad**: El sistema debe tener un sistema de roles (Cliente, Admin) (RNF-007).
*   **Fiabilidad**: El carrito de compras debe persistir incluso si se cierra el navegador (RNF-012).
*   **Mantenibilidad**: El sistema debe estar organizado en módulos independientes (Arquitectura Limpia) (RNF-021).
*   **Portabilidad**: La aplicación debe funcionar en los principales sistemas operativos y entornos de Python (RNF-023).

## 🏗️ Diagrama de Arquitectura

## 🔷 Diagrama de Clases

## Diseño de la arquitectura de software

## 🐍 Implementación en Python

### Paso 1: Estructura de Carpetas

```text
Proyecto/
├── src/
│   ├── domain/
│   │   ├── entities/
│   │   │   ├── __init__.py
│   │   │   ├── cart.py
│   │   │   ├── product.py
│   │   │   └── user.py
│   │   ├── exceptions/
│   │   │   ├── __init__.py
│   │   │   ├── product_exceptions.py
│   │   │   └── user_exceptions.py
│   │   ├── interfaces/
│   │   │   ├── __init__.py
│   │   │   ├── cart_repository.py
│   │   │   └── repositories.py
│   │   └── value_objects/
│   │       └── __init__.py
│   ├── application/
│   │   ├── dtos/
│   │   └── use_cases/
│   │       ├── cart_use_cases.py
│   │       ├── product_use_cases.py
│   │       └── user_use_cases.py
│   ├── infrastructure/
│   │   ├── config/
│   │   ├── persistence/
│   │   │   ├── database.py
│   │   │   ├── mysql_cart_repository.py
│   │   │   ├── mysql_product_repository.py
│   │   │   └── mysql_user_repository.py
│   │   └── serializers/
│   └── presentation/
│       ├── api/
│       └── cli/
│           ├── commands.py
│           └── menu.py
├── tests/
├── requirements.txt
└── main.py
```

### Paso 2: Implementación del Código

#### 2.1 Dominio - Entidades

**src/domain/entities/product.py**
```python
from typing import List, Optional

class VarianteTalla:
    def __init__(self, color_variant_id:int, size_id:int, precio:float, cantidad:int, sku:Optional[str]=None, barcode:Optional[str]=None):
        self.id = None
        self.color_variant_id = color_variant_id
        self.size_id = size_id
        self.talla_nombre = None
        self.sku = sku
        self.barcode = barcode
        self.precio = precio
        self.cantidad = cantidad

    def __str__(self):
        return f"VarianteTalla(id={self.id}, size_id={self.size_id}, precio={self.precio}, cantidad={self.cantidad})"


class VarianteColor:
    def __init__(self, producto_id:int, color_id:int, es_default:bool=False):
        self.id = None
        self.product_id = producto_id
        self.color_id = color_id
        self.color_nombre = None
        self.is_default = es_default
        self.variantes_talla: List[VarianteTalla] = []

    def agregar_variante_talla(self, variante_talla: VarianteTalla):
        self.variantes_talla.append(variante_talla)

    def __str__(self):
        return f"VarianteColor(id={self.id}, color_id={self.color_id}, tallas={len(self.variantes_talla)})"


class Producto:
    def __init__(self, nombre:str, slug:str, descripcion:str='', marca:Optional[str]=None, genero:Optional[str]=None, precio:Optional[float]=None, category_id:Optional[int]=None):
        self.id = None
        self.nombre = nombre
        self.slug = slug
        self.descripcion = descripcion
        self.marca = marca
        self.genero = genero
        self.precio = precio
        self.category_id = category_id
        self.categoria_nombre = None
        self.variantes_color: List[VarianteColor] = []

    def agregar_variante_color(self, variante_color: VarianteColor):
        self.variantes_color.append(variante_color)

    def __str__(self):
        return f"Producto(id={self.id}, nombre='{self.nombre}', variantes={len(self.variantes_color)})"
```

**src/domain/entities/user.py**
```python
from typing import List, Optional
import bcrypt

class Usuario:
    """Clase base para usuarios."""
    def __init__(self, nombre: str, correo: str, password: str, telefono: Optional[str] = None, rol: str = "customer"):
        self.id = None
        self.nombre = nombre
        self.correo = correo
        self.telefono = telefono
        self.rol = rol
        self.password_hash = None
        if password:
            self.set_password(password)
        self._telefonos: List[str] = []
        self._emails: List[str] = []

    def set_password(self, password: str):
        salt = bcrypt.gensalt()
        self.password_hash = bcrypt.hashpw(password.encode(), salt).decode()

    def verificar_password(self, password: str) -> bool:
        if not self.password_hash:
            return False
        try:
            return bcrypt.checkpw(password.encode(), self.password_hash.encode())
        except Exception:
            return False

class Cliente(Usuario):
    def __init__(self, nombre: str, correo: str, password: str, telefono: Optional[str] = None):
        super().__init__(nombre, correo, password, telefono, rol="customer")

class Admin(Usuario):
    def __init__(self, nombre: str, correo: str, password: str, telefono: Optional[str] = None):
        super().__init__(nombre, correo, password, telefono, rol="admin")
```

**src/domain/entities/cart.py**
```python
from typing import List, Optional

class CartItem:
    def __init__(self, product_id: int, quantity: int, price: float, color_variant_id: Optional[int] = None, size_variant_id: Optional[int] = None):
        self.id = None
        self.cart_id = None
        self.product_id = product_id
        self.color_variant_id = color_variant_id
        self.size_variant_id = size_variant_id
        self.quantity = quantity
        self.price = price
        self.product_name = None
        self.color_name = None
        self.size_name = None

    @property
    def subtotal(self) -> float:
        return self.price * self.quantity

class Cart:
    def __init__(self, user_id: Optional[str] = None, session_id: Optional[str] = None):
        self.id = None
        self.user_id = user_id
        self.session_id = session_id
        self.items: List[CartItem] = []

    @property
    def total(self) -> float:
        return sum(item.subtotal for item in self.items)
```

#### 2.2 Value Objects y Excepciones

**src/domain/exceptions/product_exceptions.py**
```python
class ProductException(Exception):
    """Base exception for product-related errors"""
    pass

class ProductNotFoundException(ProductException):
    """Raised when product is not found"""
    pass
```

**src/domain/exceptions/user_exceptions.py**
```python
class UserException(Exception):
    """Base exception for user-related errors"""
    pass

class InvalidUserDataException(UserException):
    """Raised when user data is invalid"""
    pass

class UserNotFoundException(UserException):
    """Raised when user is not found"""
    pass
```

#### 2.3 Interfaces de Repositorio

**src/domain/interfaces/repositories.py**
```python
from abc import ABC, abstractmethod
from typing import List, Optional
from ..entities.user import Usuario
from ..entities.product import Producto, VarianteColor, VarianteTalla

class UserRepository(ABC):
    @abstractmethod
    def save(self, user: Usuario) -> str:
        pass

    @abstractmethod
    def find_by_id(self, user_id: str) -> Optional[Usuario]:
        pass

    @abstractmethod
    def find_by_email(self, email: str) -> Optional[Usuario]:
        pass
    
    @abstractmethod
    def add_phone(self, user_id: str, phone: str) -> str:
        pass
    
    @abstractmethod
    def add_email(self, user_id: str, email: str) -> str:
        pass

class ProductRepository(ABC):
    @abstractmethod
    def save(self, product: Producto) -> int:
        pass

    @abstractmethod
    def find_all(self) -> List[Producto]:
        pass

    @abstractmethod
    def find_by_id_with_variants(self, product_id: int) -> Optional[Producto]:
        pass

    @abstractmethod
    def add_color_variant(self, product_id: int, color_id: int, is_default: bool) -> int:
        pass

    @abstractmethod
    def add_size_variant(self, color_variant_id: int, size_id: int, precio: float, cantidad: int) -> int:
        pass
    
    @abstractmethod
    def list_colors(self) -> List[dict]:
        pass

    @abstractmethod
    def list_sizes(self) -> List[dict]:
        pass

    @abstractmethod
    def list_categories(self) -> List[dict]:
        pass
```

**src/domain/interfaces/cart_repository.py**
```python
from abc import ABC, abstractmethod
from typing import Optional
from ..entities.cart import Cart, CartItem

class CartRepository(ABC):
    @abstractmethod
    def get_by_user_id(self, user_id: str) -> Optional[Cart]:
        pass

    @abstractmethod
    def create(self, user_id: str) -> int:
        pass

    @abstractmethod
    def add_item(self, cart_id: int, item: CartItem) -> int:
        pass

    @abstractmethod
    def remove_item(self, item_id: int) -> bool:
        pass
    
    @abstractmethod
    def update_item_quantity(self, item_id: int, quantity: int) -> bool:
        pass

    @abstractmethod
    def clear_cart(self, cart_id: int) -> bool:
        pass
```

#### 2.4 Casos de Uso

**src/application/use_cases/product_use_cases.py**
```python
from typing import List, Optional
from ...domain.entities.product import Producto
from ...domain.interfaces.repositories import ProductRepository

class ProductUseCases:
    def __init__(self, product_repository: ProductRepository):
        self.product_repository = product_repository

    def create_product(self, nombre: str, slug: str, descripcion: str, marca: str, genero: str, precio: float, category_id: int) -> Producto:
        product = Producto(nombre, slug, descripcion, marca, genero, precio, category_id)
        self.product_repository.save(product)
        return product

    def add_variant_to_product(self, product: Producto, color_id: int, is_default: bool, size_id: int, precio: float, cantidad: int) -> Producto:
        color_variant_id = self.product_repository.add_color_variant(product.id, color_id, is_default)
        self.product_repository.add_size_variant(color_variant_id, size_id, precio, cantidad)
        return self.get_product_details(product.id)

    def list_products(self) -> List[Producto]:
        return self.product_repository.find_all()

    def get_product_details(self, product_id: int) -> Optional[Producto]:
        return self.product_repository.find_by_id_with_variants(product_id)
```

**src/application/use_cases/user_use_cases.py**
```python
from typing import Optional, List
from ...domain.entities.user import Usuario, Cliente, Admin
from ...domain.interfaces.repositories import UserRepository
from ...domain.exceptions.user_exceptions import InvalidUserDataException, UserNotFoundException

class UserUseCases:
    def __init__(self, user_repository: UserRepository):
        self.user_repository = user_repository

    def register_user(self, nombre: str, correo: str, password: str, telefono: Optional[str] = None, rol: str = "customer") -> Usuario:
        if rol == "admin":
            user = Admin(nombre, correo, password, telefono)
        else:
            user = Cliente(nombre, correo, password, telefono)
        
        try:
            self.user_repository.save(user)
            return user
        except Exception as e:
            raise InvalidUserDataException(f"Failed to register user: {str(e)}")

    def login(self, correo: str, password: str) -> Optional[Usuario]:
        user = self.user_repository.find_by_email(correo)
        if user and user._login.authenticate(password):
            return user
        return None
```

**src/application/use_cases/cart_use_cases.py**
```python
from typing import Optional
from ...domain.entities.cart import Cart, CartItem
from ...domain.interfaces.cart_repository import CartRepository
from ...domain.interfaces.repositories import ProductRepository

class GetCartUseCase:
    def __init__(self, cart_repository: CartRepository):
        self.cart_repository = cart_repository

    def execute(self, user_id: str) -> Optional[Cart]:
        return self.cart_repository.get_by_user_id(user_id)

class AddToCartUseCase:
    def __init__(self, cart_repository: CartRepository, product_repository: ProductRepository):
        self.cart_repository = cart_repository
        self.product_repository = product_repository

    def execute(self, user_id: str, product_id: int, quantity: int, color_variant_id: int, size_variant_id: int) -> str:
        # 1. Obtener o crear carrito
        cart = self.cart_repository.get_by_user_id(user_id)
        if not cart:
            cart_id = self.cart_repository.create(user_id)
        else:
            cart_id = cart.id

        # 2. Validar producto y stock (simplificado)
        product = self.product_repository.find_by_id_with_variants(product_id)
        if not product:
            return "Producto no encontrado"

        # ... (lógica de búsqueda de precio y variante)
        price = 0.0 # Placeholder
        
        # 3. Agregar ítem
        item = CartItem(product_id=product_id, quantity=quantity, price=price, color_variant_id=color_variant_id, size_variant_id=size_variant_id)
        self.cart_repository.add_item(cart_id, item)
        return "Producto agregado al carrito exitosamente"

#### 2.4.1 DTOs

**src/application/dtos/product_dtos.py**
```python
from dataclasses import dataclass
from typing import Optional, List

@dataclass
class ProductDTO:
    id: int
    nombre: str
    slug: str
    descripcion: str
    marca: Optional[str]
    genero: Optional[str]
    precio: Optional[float]
    category_id: Optional[int]
    categoria_nombre: Optional[str]

    @staticmethod
    def from_entity(product):
        return ProductDTO(
            id=product.id,
            nombre=product.nombre,
            slug=product.slug,
            descripcion=product.descripcion,
            marca=product.marca,
            genero=product.genero,
            precio=product.precio,
            category_id=product.category_id,
            categoria_nombre=product.categoria_nombre
        )
```

**src/application/dtos/user_dtos.py**
```python
from dataclasses import dataclass
from typing import Optional, List

@dataclass
class UserDTO:
    id: str
    nombre: str
    correo: str
    telefono: Optional[str]
    rol: str
    telefonos: List[str]
    emails: List[str]

    @staticmethod
    def from_entity(user):
        return UserDTO(
            id=user.id,
            nombre=user.nombre,
            correo=user.correo,
            telefono=user.telefono,
            rol=user.rol,
            telefonos=user.telefonos,
            emails=user.emails
        )
```

**src/application/dtos/cart_dtos.py**
```python
from dataclasses import dataclass
from typing import List, Optional

@dataclass
class CartItemDTO:
    id: int
    product_id: int
    product_name: str
    color_name: str
    size_name: str
    quantity: int
    price: float
    subtotal: float

@dataclass
class CartDTO:
    id: int
    user_id: str
    items: List[CartItemDTO]
    total: float
```

```

#### 2.5 Infraestructura - Repositorios

**src/infrastructure/persistence/mysql_product_repository.py**
```python
from typing import List, Optional
from ...domain.entities.product import Producto, VarianteColor, VarianteTalla
from ...domain.interfaces.repositories import ProductRepository
from .database import Database

class MySQLProductRepository(ProductRepository):
    def __init__(self):
        self.db = Database()

    def save(self, producto: Producto) -> int:
        conn = self.db.get_connection()
        cursor = conn.cursor()
        try:
            cursor.execute(
                "INSERT INTO products (name, slug, description, brand, gender, price, category_id, created_at) VALUES (%s,%s,%s,%s,%s,%s,%s,NOW())",
                (producto.nombre, producto.slug, producto.descripcion, producto.marca, producto.genero, producto.precio, producto.category_id)
            )
            producto.id = cursor.lastrowid
            conn.commit()
            return producto.id
        except Exception as e:
            conn.rollback()
            raise e
        finally:
            cursor.close()

    def find_all(self) -> List[Producto]:
        conn = self.db.get_connection()
        cursor = conn.cursor(dictionary=True)
        cursor.execute("SELECT p.id, p.name, p.slug, p.price, p.brand, p.gender, p.category_id, c.name as category_name FROM products p LEFT JOIN categories c ON p.category_id = c.id WHERE p.is_active = 1")
        rows = cursor.fetchall()
        products = []
        for r in rows:
            p = Producto(r['name'], r['slug'], marca=r.get('brand'), genero=r.get('gender'), precio=r.get('price'), category_id=r.get('category_id'))
            p.categoria_nombre = r.get('category_name')
            p.id = r['id']
            products.append(p)
        cursor.close()
        return products
```

**src/infrastructure/persistence/mysql_user_repository.py**
```python
from typing import Optional, List
import uuid
from ...domain.entities.user import Usuario, Cliente, Admin
from ...domain.interfaces.repositories import UserRepository
from .database import Database

class MySQLUserRepository(UserRepository):
    def __init__(self):
        self.db = Database()

    def save(self, persona: Usuario) -> str:
        conn = self.db.get_connection()
        cursor = conn.cursor()
        try:
            if not persona.id:
                persona.id = uuid.uuid4().hex[:13]
            
            query = """
                INSERT INTO users (id, name, email, phone, password, role)
                VALUES (%s, %s, %s, %s, %s, %s)
                ON DUPLICATE KEY UPDATE
                name=%s, email=%s, phone=%s, password=%s, role=%s
            """
            cursor.execute(query, (
                persona.id, persona.nombre, persona.correo, persona.telefono, persona.password_hash, persona.rol,
                persona.nombre, persona.correo, persona.telefono, persona.password_hash, persona.rol
            ))
            conn.commit()
            return persona.id
        except Exception as e:
            conn.rollback()
            raise e
        finally:
            cursor.close()

    def find_by_email(self, email: str) -> Optional[Usuario]:
        with self.db.get_connection() as conn:
            with conn.cursor(dictionary=True) as cursor:
                cursor.execute("SELECT * FROM users WHERE email = %s", (email,))
                row = cursor.fetchone()
                if row:
                    return self._map_row_to_user(row)
                return None
    
    def _map_row_to_user(self, row: dict) -> Usuario:
        role = row.get('role', 'customer')
        if role == 'admin':
            persona = Admin(nombre=row['name'], correo=row['email'], password="", telefono=row.get('phone'))
        else:
            persona = Cliente(nombre=row['name'], correo=row['email'], password="", telefono=row.get('phone'))
        
        persona.id = row['id']
        persona.rol = role
        persona.password_hash = row['password']
        return persona
```

**src/infrastructure/persistence/mysql_cart_repository.py**
```python
from typing import Optional
from ...domain.entities.cart import Cart, CartItem
from ...domain.interfaces.cart_repository import CartRepository
from .database import Database

class MySQLCartRepository(CartRepository):
    def __init__(self):
        self.db = Database()

    def get_by_user_id(self, user_id: str) -> Optional[Cart]:
        conn = self.db.get_connection()
        cursor = conn.cursor(dictionary=True)
        try:
            cursor.execute("SELECT id, user_id, session_id FROM carts WHERE user_id = %s", (user_id,))
            cart_row = cursor.fetchone()
            if not cart_row:
                return None

            cart = Cart(user_id=cart_row['user_id'], session_id=cart_row['session_id'])
            cart.id = cart_row['id']
            # ... (código para cargar ítems)
            return cart
        finally:
            cursor.close()

    def create(self, user_id: str) -> int:
        conn = self.db.get_connection()
        cursor = conn.cursor()
        try:
            cursor.execute("INSERT INTO carts (user_id, created_at) VALUES (%s, NOW())", (user_id,))
            conn.commit()
            return cursor.lastrowid
        except Exception as e:
            conn.rollback()
            raise e
        finally:
            cursor.close()

#### 2.5.1 Serializers

**src/infrastructure/serializers/json_serializer.py**
```python
import json
from datetime import datetime

class JsonSerializer:
    @staticmethod
    def serialize(obj):
        if isinstance(obj, datetime):
            return obj.isoformat()
        return obj.__dict__
```

```

#### 2.6 Presentación - CLI

**src/presentation/cli/commands.py**
```python
from typing import List, Optional
from ...application.use_cases.user_use_cases import UserUseCases
from ...application.use_cases.product_use_cases import ProductUseCases
from ...application.use_cases.cart_use_cases import GetCartUseCase, AddToCartUseCase, RemoveFromCartUseCase, UpdateCartItemUseCase
from ...domain.entities.user import Usuario
from ...domain.entities.product import Producto
from ...domain.entities.cart import Cart

class CLICommands:
    def __init__(self, user_use_cases: UserUseCases, product_use_cases: ProductUseCases, 
                 get_cart_use_case: GetCartUseCase, add_to_cart_use_case: AddToCartUseCase, 
                 remove_from_cart_use_case: RemoveFromCartUseCase, update_cart_item_use_case: UpdateCartItemUseCase):
        self.user_use_cases = user_use_cases
        self.product_use_cases = product_use_cases
        self.get_cart_use_case = get_cart_use_case
        self.add_to_cart_use_case = add_to_cart_use_case
        self.remove_from_cart_use_case = remove_from_cart_use_case
        self.update_cart_item_use_case = update_cart_item_use_case

    def register_user(self, nombre: str, correo: str, password: str, telefono: Optional[str], rol: str) -> str:
        try:
            user = self.user_use_cases.register_user(nombre, correo, password, telefono, rol)
            return f"✅ Persona registrada con ID: {user.id}"
        except Exception as e:
            return f"❌ Error: {str(e)}"

    def login(self, correo: str, password: str) -> Optional[Usuario]:
        return self.user_use_cases.login(correo, password)

    def list_products(self) -> List[Producto]:
        return self.product_use_cases.list_products()

    def add_to_cart(self, user_id: str, product_id: int, quantity: int, color_variant_id: int, size_variant_id: int) -> str:
        try:
            return self.add_to_cart_use_case.execute(user_id, product_id, quantity, color_variant_id, size_variant_id)
        except Exception as e:
            return f"❌ Error: {str(e)}"
```

**src/presentation/cli/menu.py**
```python
class CLIMenu:
    def __init__(self, commands):
        self.commands = commands
        self.usuario_actual = None

    def display_main_menu(self):
        print("\n" + "="*50)
        print("📋 MENÚ PRINCIPAL")
        print("="*50)
        print("1. 👉Registrar nueva persona")
        print("2. 😎Iniciar sesión")
        print("3. 👣Salir")

    def run(self):
        while True:
            if self.usuario_actual is None:
                self.display_main_menu()
                choice = input("Seleccione opción: ")
                if choice == "1":
                    self._register_user()
                elif choice == "2":
                    self._login()
                elif choice == "3":
                    break
            else:
                self.display_user_menu()
```

#### 2.7 Archivo Principal

**main.py**
```python
import sys
import os
from src.infrastructure.persistence.mysql_user_repository import MySQLUserRepository
from src.infrastructure.persistence.mysql_product_repository import MySQLProductRepository
from src.infrastructure.persistence.mysql_cart_repository import MySQLCartRepository
from src.application.use_cases.user_use_cases import UserUseCases
from src.application.use_cases.product_use_cases import ProductUseCases
from src.application.use_cases.cart_use_cases import GetCartUseCase, AddToCartUseCase, RemoveFromCartUseCase, UpdateCartItemUseCase
from src.presentation.cli.commands import CLICommands
from src.presentation.cli.menu import CLIMenu

def main():
    # Inicializar repositorios
    user_repository = MySQLUserRepository()
    product_repository = MySQLProductRepository()
    cart_repository = MySQLCartRepository()

    # Inicializar casos de uso
    user_use_cases = UserUseCases(user_repository)
    product_use_cases = ProductUseCases(product_repository)
    get_cart_use_case = GetCartUseCase(cart_repository)
    add_to_cart_use_case = AddToCartUseCase(cart_repository, product_repository)
    remove_from_cart_use_case = RemoveFromCartUseCase(cart_repository)
    update_cart_item_use_case = UpdateCartItemUseCase(cart_repository)
    
    # Inicializar CLI
    commands = CLICommands(
        user_use_cases, 
        product_use_cases, 
        get_cart_use_case, 
        add_to_cart_use_case, 
        remove_from_cart_use_case, 
        update_cart_item_use_case
    )
    menu = CLIMenu(commands)

    # Ejecutar la aplicación
    menu.run()

if __name__ == "__main__":
    main()
```

#### 2.8 Requirements

**requirements.txt**
```text
mysql-connector-python
python-dotenv
bcrypt
```

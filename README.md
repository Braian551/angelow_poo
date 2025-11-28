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
*   **Editar perfil de usuario**: El sistema debe permitir a los usuarios modificar su nombre y teléfono, pero no su correo electrónico (RF-004).
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

```
📦 Proyecto
├── 🗂️ src
│   ├── 🗂️ domain
│   │   ├── entities/
│   │   ├── value_objects/
│   │   ├── exceptions/
│   │   └── interfaces/
│   ├── 🗂️ application
│   │   ├── use_cases/
│   │   ├── dtos/
│   │   └── interfaces/
│   ├── 🗂️ infrastructure
│   │   ├── persistence/
│   │   ├── serializers/
│   │   └── config/
│   └── 🗂️ presentation
│       ├── cli/
│       └── api/
```
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
    def __init__(self, id_variante_color: int, id_talla: int, precio: float, cantidad: int, sku: Optional[str] = None, codigo_barras: Optional[str] = None):
        self.id = None
        self.id_variante_color = id_variante_color
        self.id_talla = id_talla
        self.nombre_talla = None
        self.sku = sku
        self.codigo_barras = codigo_barras
        self.precio = precio
        self.cantidad = cantidad

    def __str__(self):
        return f"VarianteTalla(id={self.id}, id_talla={self.id_talla}, precio={self.precio}, cantidad={self.cantidad})"

class VarianteColor:
    def __init__(self, id_producto: int, id_color: int, es_defecto: bool = False):
        self.id = None
        self.id_producto = id_producto
        self.id_color = id_color
        self.nombre_color = None
        self.es_defecto = es_defecto
        self.variantes_talla: List[VarianteTalla] = []

    def agregar_variante_talla(self, variante_talla: VarianteTalla):
        self.variantes_talla.append(variante_talla)

    def __str__(self):
        return f"VarianteColor(id={self.id}, id_color={self.id_color}, tallas={len(self.variantes_talla)})"

class Producto:
    def __init__(self, nombre: str, slug: str, descripcion: str = '', marca: Optional[str] = None, genero: Optional[str] = None, precio: Optional[float] = None, id_categoria: Optional[int] = None):
        self.id = None
        self.nombre = nombre
        self.slug = slug
        self.descripcion = descripcion
        self.marca = marca
        self.genero = genero
        self.precio = precio
        self.id_categoria = id_categoria
        self.nombre_categoria = None
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
    """Clase base para representar a los usuarios del sistema."""
    def __init__(self, nombre: str, correo: str, contrasena: str, telefono: Optional[str] = None, rol: str = "customer"):
        self.id = None
        self.nombre = nombre
        self.correo = correo
        self.telefono = telefono
        self.rol = rol
        self.hash_contrasena = None
        if contrasena:
            self.establecer_contrasena(contrasena)
        self._telefonos: List[str] = []
        self._emails: List[str] = []

    def establecer_contrasena(self, contrasena: str):
        salt = bcrypt.gensalt()
        self.hash_contrasena = bcrypt.hashpw(contrasena.encode(), salt).decode()

    def verificar_contrasena(self, contrasena: str) -> bool:
        if not self.hash_contrasena:
            return False
        try:
            return bcrypt.checkpw(contrasena.encode(), self.hash_contrasena.encode())
        except Exception:
            return False

    class InicioSesion:
        def __init__(self, correo: str, hash_contrasena: str):
            self.correo = correo
            self.hash_contrasena = hash_contrasena
            self._esta_autenticado = False

        def autenticar(self, contrasena: str) -> bool:
            self._esta_autenticado = bcrypt.checkpw(contrasena.encode(), self.hash_contrasena.encode())
            return self._esta_autenticado

        @property
        def esta_autenticado(self) -> bool:
            return self._esta_autenticado

class Cliente(Usuario):
    def __init__(self, nombre: str, correo: str, contrasena: str, telefono: Optional[str] = None):
        super().__init__(nombre, correo, contrasena, telefono, rol="customer")
        self._inicio_sesion = self.InicioSesion(self.correo, self.hash_contrasena)

class Admin(Usuario):
    def __init__(self, nombre: str, correo: str, contrasena: str, telefono: Optional[str] = None):
        super().__init__(nombre, correo, contrasena, telefono, rol="admin")
```

**src/domain/entities/cart.py**
```python
from typing import List, Optional

class ItemCarrito:
    def __init__(self, id_producto: int, cantidad: int, precio: float, id_variante_color: Optional[int] = None, id_variante_talla: Optional[int] = None):
        self.id = None
        self.id_carrito = None
        self.id_producto = id_producto
        self.id_variante_color = id_variante_color
        self.id_variante_talla = id_variante_talla
        self.cantidad = cantidad
        self.precio = precio
        # Metadatos opcionales para visualización
        self.nombre_producto = None
        self.nombre_color = None
        self.nombre_talla = None

    @property
    def subtotal(self) -> float:
        return self.precio * self.cantidad

    def __str__(self):
        return f"ItemCarrito(producto={self.id_producto}, cant={self.cantidad}, subtotal={self.subtotal})"

class Carrito:
    def __init__(self, id_usuario: Optional[str] = None, id_sesion: Optional[str] = None):
        self.id = None
        self.id_usuario = id_usuario
        self.id_sesion = id_sesion
        self.items: List[ItemCarrito] = []

    @property
    def total(self) -> float:
        return sum(item.subtotal for item in self.items)

    def agregar_item(self, item: ItemCarrito):
        self.items.append(item)

    def __str__(self):
        return f"Carrito(id={self.id}, usuario={self.id_usuario}, items={len(self.items)}, total={self.total})"
```

#### 2.2 Value Objects y Excepciones

**src/domain/exceptions/product_exceptions.py**
```python
class ExcepcionProducto(Exception):
    """Excepción base para errores relacionados con productos"""
    pass

class ExcepcionProductoNoEncontrado(ExcepcionProducto):
    """Se lanza cuando no se encuentra el producto"""
    pass
```

**src/domain/exceptions/user_exceptions.py**
```python
class ExcepcionUsuario(Exception):
    """Excepción base para errores relacionados con usuarios"""
    pass

class ExcepcionDatosUsuarioInvalidos(ExcepcionUsuario):
    """Se lanza cuando los datos del usuario son inválidos"""
    pass

class ExcepcionUsuarioNoEncontrado(ExcepcionUsuario):
    """Se lanza cuando no se encuentra el usuario"""
    pass
```

#### 2.3 Interfaces de Repositorio

**src/domain/interfaces/repositories.py**
```python
from abc import ABC, abstractmethod
from typing import List, Optional
from ..entities.user import Usuario
from ..entities.product import Producto, VarianteColor, VarianteTalla

class RepositorioUsuario(ABC):
    @abstractmethod
    def guardar(self, usuario: Usuario) -> str:
        pass

    @abstractmethod
    def buscar_por_id(self, id_usuario: str) -> Optional[Usuario]:
        pass

    @abstractmethod
    def buscar_por_email(self, email: str) -> Optional[Usuario]:
        pass
    
    @abstractmethod
    def agregar_telefono(self, id_usuario: str, telefono: str) -> str:
        pass
    
    @abstractmethod
    def actualizar(self, usuario: Usuario) -> str:
        pass

class RepositorioProducto(ABC):
    @abstractmethod
    def guardar(self, producto: Producto) -> int:
        pass

    @abstractmethod
    def buscar_todos(self) -> List[Producto]:
        pass

    @abstractmethod
    def buscar_por_id_con_variantes(self, id_producto: int) -> Optional[Producto]:
        pass

    @abstractmethod
    def agregar_variante_color(self, id_producto: int, id_color: int, es_defecto: bool) -> int:
        pass

    @abstractmethod
    def agregar_variante_talla(self, id_variante_color: int, id_talla: int, precio: float, cantidad: int) -> int:
        pass
    
    @abstractmethod
    def listar_colores(self) -> List[dict]:
        pass

    @abstractmethod
    def listar_tallas(self) -> List[dict]:
        pass

    @abstractmethod
    def listar_categorias(self) -> List[dict]:
        pass
```

**src/domain/interfaces/cart_repository.py**
```python
from abc import ABC, abstractmethod
from typing import Optional
from ..entities.cart import Carrito, ItemCarrito

class RepositorioCarrito(ABC):
    @abstractmethod
    def obtener_por_id_usuario(self, id_usuario: str) -> Optional[Carrito]:
        pass

    @abstractmethod
    def crear(self, id_usuario: str) -> int:
        pass

    @abstractmethod
    def agregar_item(self, id_carrito: int, item: ItemCarrito) -> int:
        pass

    @abstractmethod
    def eliminar_item(self, id_item: int) -> bool:
        pass
    
    @abstractmethod
    def actualizar_cantidad_item(self, id_item: int, cantidad: int) -> bool:
        pass

    @abstractmethod
    def vaciar_carrito(self, id_carrito: int) -> bool:
        pass
```

#### 2.4 Casos de Uso

**src/application/use_cases/product_use_cases.py**
```python
from typing import List, Optional
from ...domain.entities.product import Producto, VarianteColor, VarianteTalla
from ...domain.interfaces.repositories import RepositorioProducto
from ...domain.exceptions.product_exceptions import ExcepcionProductoNoEncontrado

class CasosUsoProducto:
    def __init__(self, repositorio_producto: RepositorioProducto):
        self.repositorio_producto = repositorio_producto

    def crear_producto(self, nombre: str, slug: str, descripcion: str, marca: str, genero: str, precio: float, id_categoria: int) -> Producto:
        producto = Producto(nombre, slug, descripcion, marca, genero, precio, id_categoria)
        self.repositorio_producto.guardar(producto)
        return producto

    def agregar_variante_a_producto(self, producto: Producto, id_color: int, es_defecto: bool, id_talla: int, precio: float, cantidad: int) -> Producto:
        # Agregar variante de color
        id_variante_color = self.repositorio_producto.agregar_variante_color(producto.id, id_color, es_defecto)
        
        # Agregar variante de talla
        self.repositorio_producto.agregar_variante_talla(id_variante_color, id_talla, precio, cantidad)
        
        # Devolver producto actualizado
        return self.obtener_detalles_producto(producto.id)

    def agregar_variante_color(self, id_producto: int, id_color: int, es_defecto: bool) -> int:
        return self.repositorio_producto.agregar_variante_color(id_producto, id_color, es_defecto)

    def agregar_variante_talla(self, id_variante_color: int, id_talla: int, precio: float, cantidad: int) -> int:
        return self.repositorio_producto.agregar_variante_talla(id_variante_color, id_talla, precio, cantidad)

    def listar_productos(self) -> List[Producto]:
        return self.repositorio_producto.buscar_todos()

    def obtener_detalles_producto(self, id_producto: int) -> Optional[Producto]:
        return self.repositorio_producto.buscar_por_id_con_variantes(id_producto)

    def listar_categorias(self) -> List[dict]:
        return self.repositorio_producto.listar_categorias()

    def listar_colores(self) -> List[dict]:
        return self.repositorio_producto.listar_colores()

    def listar_tallas(self) -> List[dict]:
        return self.repositorio_producto.listar_tallas()
```

**src/application/use_cases/user_use_cases.py**
```python
from typing import Optional, List
from ...domain.entities.user import Usuario, Cliente, Admin
from ...domain.interfaces.repositories import RepositorioUsuario
from ...domain.exceptions.user_exceptions import ExcepcionDatosUsuarioInvalidos, ExcepcionUsuarioNoEncontrado

class CasosUsoUsuario:
    def __init__(self, repositorio_usuario: RepositorioUsuario):
        self.repositorio_usuario = repositorio_usuario

    def registrar_usuario(self, nombre: str, correo: str, contrasena: str, telefono: Optional[str] = None, rol: str = "customer") -> Usuario:
        if rol == "admin":
            usuario = Admin(nombre, correo, contrasena, telefono)
        else:
            usuario = Cliente(nombre, correo, contrasena, telefono)
        
        try:
            self.repositorio_usuario.guardar(usuario)
            return usuario
        except Exception as e:
            raise ExcepcionDatosUsuarioInvalidos(f"Error al registrar usuario: {str(e)}")

    def iniciar_sesion(self, correo: str, contrasena: str) -> Optional[Usuario]:
        usuario = self.repositorio_usuario.buscar_por_email(correo)
        if usuario and usuario._inicio_sesion.autenticar(contrasena):
            return usuario
        return None

    def agregar_telefono(self, id_usuario: str, telefono: str) -> Usuario:
        usuario = self.repositorio_usuario.buscar_por_id(id_usuario)
        if not usuario:
            raise ExcepcionUsuarioNoEncontrado(f"Usuario {id_usuario} no encontrado")
        
        usuario.agregar_telefono(telefono)
        self.repositorio_usuario.agregar_telefono(id_usuario, telefono)
        return usuario

    def actualizar_perfil(self, id_usuario: str, nombre: str, telefono: str) -> Usuario:
        usuario = self.repositorio_usuario.buscar_por_id(id_usuario)
        if not usuario:
            raise ExcepcionUsuarioNoEncontrado(f"Usuario {id_usuario} no encontrado")
        
        usuario.nombre = nombre
        usuario.telefono = telefono
        # Actualizar en repositorio
        self.repositorio_usuario.actualizar(usuario)
        return usuario
```

**src/application/use_cases/cart_use_cases.py**
```python
from typing import Optional
from ...domain.entities.cart import Carrito, ItemCarrito
from ...domain.interfaces.cart_repository import RepositorioCarrito
from ...domain.interfaces.repositories import RepositorioProducto

class CasoUsoObtenerCarrito:
    def __init__(self, repositorio_carrito: RepositorioCarrito):
        self.repositorio_carrito = repositorio_carrito

    def ejecutar(self, id_usuario: str) -> Optional[Carrito]:
        return self.repositorio_carrito.obtener_por_id_usuario(id_usuario)

class CasoUsoAgregarAlCarrito:
    def __init__(self, repositorio_carrito: RepositorioCarrito, repositorio_producto: RepositorioProducto):
        self.repositorio_carrito = repositorio_carrito
        self.repositorio_producto = repositorio_producto

    def ejecutar(self, id_usuario: str, id_producto: int, cantidad: int, id_variante_color: int, id_variante_talla: int) -> str:
        # 1. Obtener o crear carrito
        carrito = self.repositorio_carrito.obtener_por_id_usuario(id_usuario)
        if not carrito:
            id_carrito = self.repositorio_carrito.crear(id_usuario)
        else:
            id_carrito = carrito.id

        # 2. Validar producto y stock
        
        producto = self.repositorio_producto.buscar_por_id_con_variantes(id_producto)
        if not producto:
            return "Producto no encontrado"

        # Encontrar el precio de la variante específica
        precio = 0.0
        encontrado = False
        for var_color in producto.variantes_color:
            if var_color.id == id_variante_color:
                for var_talla in var_color.variantes_talla:
                    if var_talla.id == id_variante_talla:
                        precio = var_talla.precio
                        encontrado = True
                        break
            if encontrado: break
        
        if not encontrado:
            return "Variante no encontrada"

        # 3. Agregar ítem
        item = ItemCarrito(
            id_producto=id_producto,
            cantidad=cantidad,
            precio=precio,
            id_variante_color=id_variante_color,
            id_variante_talla=id_variante_talla
        )
        
        self.repositorio_carrito.agregar_item(id_carrito, item)
        return "Producto agregado al carrito exitosamente"

class CasoUsoEliminarDelCarrito:
    def __init__(self, repositorio_carrito: RepositorioCarrito):
        self.repositorio_carrito = repositorio_carrito

    def ejecutar(self, id_item: int) -> str:
        exito = self.repositorio_carrito.eliminar_item(id_item)
        return "Producto eliminado del carrito" if exito else "Error al eliminar producto"

class CasoUsoActualizarItemCarrito:
    def __init__(self, repositorio_carrito: RepositorioCarrito):
        self.repositorio_carrito = repositorio_carrito

    def ejecutar(self, id_item: int, cantidad: int) -> str:
        exito = self.repositorio_carrito.actualizar_cantidad_item(id_item, cantidad)
        return "Cantidad actualizada" if exito else "Error al actualizar cantidad"

#### 2.4.1 DTOs

**src/application/dtos/product_dtos.py**
```python
from dataclasses import dataclass
from typing import Optional, List

@dataclass
class ProductoDTO:
    id: int
    nombre: str
    slug: str
    descripcion: str
    marca: Optional[str]
    genero: Optional[str]
    precio: Optional[float]
    id_categoria: Optional[int]
    nombre_categoria: Optional[str]

    @staticmethod
    def desde_entidad(producto):
        return ProductoDTO(
            id=producto.id,
            nombre=producto.nombre,
            slug=producto.slug,
            descripcion=producto.descripcion,
            marca=producto.marca,
            genero=producto.genero,
            precio=producto.precio,
            id_categoria=producto.id_categoria,
            nombre_categoria=producto.nombre_categoria
        )
```

**src/application/dtos/user_dtos.py**
```python
from dataclasses import dataclass
from typing import Optional, List

@dataclass
class UsuarioDTO:
    id: str
    nombre: str
    correo: str
    telefono: Optional[str]
    rol: str
    telefonos: List[str]
    emails: List[str]

    @staticmethod
    def desde_entidad(usuario):
        return UsuarioDTO(
            id=usuario.id,
            nombre=usuario.nombre,
            correo=usuario.correo,
            telefono=usuario.telefono,
            rol=usuario.rol,
            telefonos=usuario.telefonos,
            emails=usuario.emails
        )
```

**src/application/dtos/cart_dtos.py**
```python
from dataclasses import dataclass
from typing import List, Optional

@dataclass
class ItemCarritoDTO:
    id: int
    id_producto: int
    nombre_producto: str
    nombre_color: str
    nombre_talla: str
    cantidad: int
    precio: float
    subtotal: float

@dataclass
class CarritoDTO:
    id: int
    id_usuario: str
    items: List[ItemCarritoDTO]
    total: float
```

```

#### 2.5 Infraestructura - Repositorios

**src/infrastructure/persistence/mysql_product_repository.py**
```python
from typing import List, Optional
from ...domain.entities.product import Producto, VarianteColor, VarianteTalla
from ...domain.interfaces.repositories import RepositorioProducto
from .database import BaseDeDatos

class RepositorioProductoMySQL(RepositorioProducto):
    def __init__(self):
        self.bd = BaseDeDatos()

    def guardar(self, producto: Producto) -> int:
        conexion = self.bd.obtener_conexion()
        cursor = conexion.cursor()
        try:
            cursor.execute(
                "INSERT INTO products (name, slug, description, brand, gender, price, category_id, created_at) VALUES (%s,%s,%s,%s,%s,%s,%s,NOW())",
                (producto.nombre, producto.slug, producto.descripcion, producto.marca, producto.genero, producto.precio, producto.id_categoria)
            )
            producto.id = cursor.lastrowid

            for var_color in producto.variantes_color:
                cursor.execute("INSERT INTO product_color_variants (product_id, color_id, is_default) VALUES (%s,%s,%s)", (producto.id, var_color.id_color, 1 if var_color.es_defecto else 0))
                var_color.id = cursor.lastrowid
                for var_talla in var_color.variantes_talla:
                    cursor.execute(
                        "INSERT INTO product_size_variants (color_variant_id, size_id, sku, barcode, price, compare_price, quantity, is_active) VALUES (%s,%s,%s,%s,%s,%s,%s,1)",
                        (var_color.id, var_talla.id_talla, var_talla.sku, var_talla.codigo_barras, var_talla.precio, None, var_talla.cantidad)
                    )
                    var_talla.id = cursor.lastrowid
            conexion.commit()
            return producto.id
        except Exception as e:
            conexion.rollback()
            raise e
        finally:
            cursor.close()

    def buscar_todos(self) -> List[Producto]:
        conexion = self.bd.obtener_conexion()
        cursor = conexion.cursor(dictionary=True)
        cursor.execute("SELECT p.id, p.name, p.slug, p.price, p.brand, p.gender, p.category_id, c.name as category_name FROM products p LEFT JOIN categories c ON p.category_id = c.id WHERE p.is_active = 1")
        filas = cursor.fetchall()
        productos = []
        for r in filas:
            p = Producto(r['name'], r['slug'], marca=r.get('brand'), genero=r.get('gender'), precio=r.get('price'), id_categoria=r.get('category_id'))
            p.nombre_categoria = r.get('category_name')
            p.id = r['id']
            productos.append(p)
        cursor.close()
        return productos
```

**src/infrastructure/persistence/mysql_user_repository.py**
```python
from typing import Optional, List
import uuid
from ...domain.entities.user import Usuario, Cliente, Admin
from ...domain.interfaces.repositories import RepositorioUsuario
from .database import BaseDeDatos

class RepositorioUsuarioMySQL(RepositorioUsuario):
    def __init__(self):
        self.bd = BaseDeDatos()

    def guardar(self, persona: Usuario) -> str:
        conexion = self.bd.obtener_conexion()
        cursor = conexion.cursor()
        
        try:
            if not persona.id:
                persona.id = uuid.uuid4().hex[:13]
            
            consulta = """
                INSERT INTO users (id, name, email, phone, password, role)
                VALUES (%s, %s, %s, %s, %s, %s)
                ON DUPLICATE KEY UPDATE
                name=%s, email=%s, phone=%s, password=%s, role=%s
            """
            cursor.execute(consulta, (
                persona.id, persona.nombre, persona.correo, persona.telefono, persona.hash_contrasena, persona.rol,
                persona.nombre, persona.correo, persona.telefono, persona.hash_contrasena, persona.rol
            ))
            conexion.commit()
            return persona.id
        except Exception as e:
            conexion.rollback()
            raise e
        finally:
            cursor.close()

    def buscar_por_email(self, email: str) -> Optional[Usuario]:
        with self.bd.obtener_conexion() as conexion:
            with conexion.cursor(dictionary=True) as cursor:
                cursor.execute("SELECT * FROM users WHERE email = %s", (email,))
                fila = cursor.fetchone()
                if fila:
                    return self._mapear_fila_a_usuario(fila)
                return None
    
    def _mapear_fila_a_usuario(self, fila: dict) -> Usuario:
        rol = fila.get('role', 'customer')
        if rol == 'admin':
            persona = Admin(nombre=fila['name'], correo=fila['email'], contrasena="", telefono=fila.get('phone'))
        else:
            persona = Cliente(nombre=fila['name'], correo=fila['email'], contrasena="", telefono=fila.get('phone'))
        
        persona.id = fila['id']
        persona.rol = rol
        persona.hash_contrasena = fila['password']
        # Re-inicializar ayudante de login con hash cargado
        persona._inicio_sesion = persona.InicioSesion(persona.correo, persona.hash_contrasena)
        
    def agregar_telefono(self, id_usuario: str, telefono: str) -> str:
        conexion = self.bd.obtener_conexion()
        cursor = conexion.cursor()
        cursor.execute("UPDATE users SET phone = %s WHERE id = %s", (telefono, id_usuario))
        conexion.commit()
        cursor.close()
        return id_usuario

    def actualizar(self, usuario: Usuario) -> str:
        conexion = self.bd.obtener_conexion()
        cursor = conexion.cursor()
        try:
            cursor.execute("UPDATE users SET name = %s, phone = %s WHERE id = %s", (usuario.nombre, usuario.telefono, usuario.id))
            conexion.commit()
            return usuario.id
        except Exception as e:
            conexion.rollback()
            raise e
        finally:
            cursor.close()
```

**src/infrastructure/persistence/mysql_cart_repository.py**
```python
from typing import Optional
from ...domain.entities.cart import Carrito, ItemCarrito
from ...domain.interfaces.cart_repository import RepositorioCarrito
from .database import BaseDeDatos

class RepositorioCarritoMySQL(RepositorioCarrito):
    def __init__(self):
        self.bd = BaseDeDatos()

    def obtener_por_id_usuario(self, id_usuario: str) -> Optional[Carrito]:
        conexion = self.bd.obtener_conexion()
        cursor = conexion.cursor(dictionary=True)
        try:
            # Obtener carrito
            cursor.execute("SELECT id, user_id, session_id FROM carts WHERE user_id = %s", (id_usuario,))
            fila_carrito = cursor.fetchone()
            
            if not fila_carrito:
                return None

            carrito = Carrito(id_usuario=fila_carrito['user_id'], id_sesion=fila_carrito['session_id'])
            carrito.id = fila_carrito['id']

            # Obtener ítems
            cursor.execute("""
                SELECT ci.id, ci.product_id, ci.quantity, ci.color_variant_id, ci.size_variant_id,
                       psv.price, p.name as product_name, c.name as color_name, s.name as size_name
                FROM cart_items ci
                JOIN products p ON ci.product_id = p.id
                LEFT JOIN product_size_variants psv ON ci.size_variant_id = psv.id
                LEFT JOIN product_color_variants pcv ON ci.color_variant_id = pcv.id
                LEFT JOIN colors c ON pcv.color_id = c.id
                LEFT JOIN sizes s ON psv.size_id = s.id
                WHERE ci.cart_id = %s
            """, (carrito.id,))
            
            filas_items = cursor.fetchall()
            for fila in filas_items:
                # Precio de respaldo si no se encuentra la variante (no debería suceder con integridad estricta)
                precio = float(fila['price']) if fila['price'] else 0.0
                item = ItemCarrito(
                    id_producto=fila['product_id'],
                    cantidad=fila['quantity'],
                    precio=precio,
                    id_variante_color=fila['color_variant_id'],
                    id_variante_talla=fila['size_variant_id']
                )
                item.id = fila['id']
                item.id_carrito = carrito.id
                item.nombre_producto = fila['product_name']
                item.nombre_color = fila['color_name']
                item.nombre_talla = fila['size_name']
                carrito.agregar_item(item)
            
            return carrito
        finally:
            cursor.close()

    def crear(self, id_usuario: str) -> int:
        conexion = self.bd.obtener_conexion()
        cursor = conexion.cursor()
        try:
            cursor.execute("INSERT INTO carts (user_id, created_at) VALUES (%s, NOW())", (id_usuario,))
            conexion.commit()
            return cursor.lastrowid
        except Exception as e:
            conexion.rollback()
            raise e
        finally:
            cursor.close()

    def agregar_item(self, id_carrito: int, item: ItemCarrito) -> int:
        conexion = self.bd.obtener_conexion()
        cursor = conexion.cursor(buffered=True)
        try:
            # Verificar si el ítem existe para actualizar cantidad
            cursor.execute("""
                SELECT id, quantity FROM cart_items 
                WHERE cart_id = %s AND product_id = %s AND color_variant_id = %s AND size_variant_id = %s
            """, (id_carrito, item.id_producto, item.id_variante_color, item.id_variante_talla))
            
            existente = cursor.fetchone()
            
            if existente:
                nueva_cantidad = existente[1] + item.cantidad
                cursor.execute("UPDATE cart_items SET quantity = %s WHERE id = %s", (nueva_cantidad, existente[0]))
                conexion.commit()
                return existente[0]
            else:
                cursor.execute("""
                    INSERT INTO cart_items (cart_id, product_id, color_variant_id, size_variant_id, quantity, created_at) 
                    VALUES (%s, %s, %s, %s, %s, NOW())
                """, (id_carrito, item.id_producto, item.id_variante_color, item.id_variante_talla, item.cantidad))
                conexion.commit()
                return cursor.lastrowid
        except Exception as e:
            conexion.rollback()
            raise e
        finally:
            cursor.close()

    def eliminar_item(self, id_item: int) -> bool:
        conexion = self.bd.obtener_conexion()
        cursor = conexion.cursor()
        try:
            cursor.execute("DELETE FROM cart_items WHERE id = %s", (id_item,))
            conexion.commit()
            return cursor.rowcount > 0
        except Exception as e:
            conexion.rollback()
            raise e
        finally:
            cursor.close()

    def actualizar_cantidad_item(self, id_item: int, cantidad: int) -> bool:
        conexion = self.bd.obtener_conexion()
        cursor = conexion.cursor()
        try:
            if cantidad <= 0:
                return self.eliminar_item(id_item)
            
            cursor.execute("UPDATE cart_items SET quantity = %s WHERE id = %s", (cantidad, id_item))
            conexion.commit()
            return cursor.rowcount > 0
        except Exception as e:
            conexion.rollback()
            raise e
        finally:
            cursor.close()

    def vaciar_carrito(self, id_carrito: int) -> bool:
        conexion = self.bd.obtener_conexion()
        cursor = conexion.cursor()
        try:
            cursor.execute("DELETE FROM cart_items WHERE cart_id = %s", (id_carrito,))
            conexion.commit()
            return True
        except Exception as e:
            conexion.rollback()
            raise e
        finally:
            cursor.close()

#### 2.5.1 Serializers

**src/infrastructure/serializers/json_serializer.py**
```python
import json
from datetime import datetime

class SerializadorJson:
    @staticmethod
    def serializar(obj):
        if isinstance(obj, datetime):
            return obj.isoformat()
        return obj.__dict__
```

```

#### 2.6 Presentación - CLI

**src/presentation/cli/commands.py**
```python
from typing import List, Optional
from ...application.use_cases.user_use_cases import CasosUsoUsuario
from ...application.use_cases.product_use_cases import CasosUsoProducto
from ...application.use_cases.cart_use_cases import CasoUsoObtenerCarrito, CasoUsoAgregarAlCarrito, CasoUsoEliminarDelCarrito, CasoUsoActualizarItemCarrito
from ...domain.entities.user import Usuario
from ...domain.entities.product import Producto
from ...domain.entities.cart import Carrito

class ComandosCLI:
    def __init__(self, casos_uso_usuario: CasosUsoUsuario, casos_uso_producto: CasosUsoProducto, 
                 caso_uso_obtener_carrito: CasoUsoObtenerCarrito, caso_uso_agregar_al_carrito: CasoUsoAgregarAlCarrito, 
                 caso_uso_eliminar_del_carrito: CasoUsoEliminarDelCarrito, caso_uso_actualizar_item_carrito: CasoUsoActualizarItemCarrito):
        self.casos_uso_usuario = casos_uso_usuario
        self.casos_uso_producto = casos_uso_producto
        self.caso_uso_obtener_carrito = caso_uso_obtener_carrito
        self.caso_uso_agregar_al_carrito = caso_uso_agregar_al_carrito
        self.caso_uso_eliminar_del_carrito = caso_uso_eliminar_del_carrito
        self.caso_uso_actualizar_item_carrito = caso_uso_actualizar_item_carrito

    def registrar_usuario(self, nombre: str, correo: str, contrasena: str, telefono: Optional[str], rol: str) -> str:
        try:
            usuario = self.casos_uso_usuario.registrar_usuario(nombre, correo, contrasena, telefono, rol)
            return f"✅ Persona registrada con ID: {usuario.id}"
        except Exception as e:
            return f"❌ Error: {str(e)}"

    def iniciar_sesion(self, correo: str, contrasena: str) -> Optional[Usuario]:
        return self.casos_uso_usuario.iniciar_sesion(correo, contrasena)

    def actualizar_perfil(self, id_usuario: str, nombre: str, telefono: str) -> str:
        try:
            self.casos_uso_usuario.actualizar_perfil(id_usuario, nombre, telefono)
            return "✅ Perfil actualizado"
        except Exception as e:
            return f"❌ Error: {str(e)}"

    def listar_productos(self) -> List[Producto]:
        return self.casos_uso_producto.listar_productos()

    def agregar_al_carrito(self, id_usuario: str, id_producto: int, cantidad: int, id_variante_color: int, id_variante_talla: int) -> str:
        try:
            return self.caso_uso_agregar_al_carrito.ejecutar(id_usuario, id_producto, cantidad, id_variante_color, id_variante_talla)
        except Exception as e:
            return f"❌ Error: {str(e)}"
```

**src/presentation/cli/menu.py**
```python
class MenuCLI:
    def __init__(self, comandos):
        self.comandos = comandos
        self.usuario_actual = None

    def mostrar_menu_principal(self):
        print("\n" + "="*50)
        print("📋 MENÚ PRINCIPAL")
        print("="*50)
        print("1. 👉Registrar nueva persona")
        print("2. 😎Iniciar sesión")
        print("3. 👣Salir")

    def ejecutar(self):
        while True:
            if self.usuario_actual is None:
                self.mostrar_menu_principal()
                opcion = input("Seleccione opción: ")
                if opcion == "1":
                    self._registrar_usuario()
                elif opcion == "2":
                    self._iniciar_sesion()
                elif opcion == "3":
                    break
            else:
                self.mostrar_menu_usuario()
                opcion = input("Seleccione opción: ")
                if opcion == "1":
                    self._mostrar_info()
                elif opcion == "2":
                    self._gestionar_productos()
                elif opcion == "3":
                    self._agregar_telefono()
                elif opcion == "4":
                    self._editar_perfil()
                elif opcion == "5":
                    self.usuario_actual = None
                    print("🔒 Sesión cerrada")
                elif opcion == "6":
                    self._menu_carrito()

    def _editar_perfil(self):
        print("\n--- EDITAR PERFIL ---")
        print(f"Nombre actual: {self.usuario_actual.nombre}")
        print(f"Teléfono actual: {self.usuario_actual.telefono}")
        
        nuevo_nombre = input("Nuevo nombre (dejar vacío para mantener): ") or self.usuario_actual.nombre
        nuevo_telefono = input("Nuevo teléfono (dejar vacío para mantener): ") or self.usuario_actual.telefono
        
        print(self.comandos.actualizar_perfil(self.usuario_actual.id, nuevo_nombre, nuevo_telefono))
        # Actualizar instancia local
        self.usuario_actual.nombre = nuevo_nombre
        self.usuario_actual.telefono = nuevo_telefono
```

#### 2.7 Archivo Principal

**main.py**
```python
import sys
import os
from src.infrastructure.persistence.mysql_user_repository import RepositorioUsuarioMySQL
from src.infrastructure.persistence.mysql_product_repository import RepositorioProductoMySQL
from src.infrastructure.persistence.mysql_cart_repository import RepositorioCarritoMySQL
from src.application.use_cases.user_use_cases import CasosUsoUsuario
from src.application.use_cases.product_use_cases import CasosUsoProducto
from src.application.use_cases.cart_use_cases import CasoUsoObtenerCarrito, CasoUsoAgregarAlCarrito, CasoUsoEliminarDelCarrito, CasoUsoActualizarItemCarrito
from src.presentation.cli.commands import ComandosCLI
from src.presentation.cli.menu import MenuCLI

def main():
    # Inicializar repositorios
    repositorio_usuario = RepositorioUsuarioMySQL()
    repositorio_producto = RepositorioProductoMySQL()
    repositorio_carrito = RepositorioCarritoMySQL()

    # Inicializar casos de uso
    casos_uso_usuario = CasosUsoUsuario(repositorio_usuario)
    casos_uso_producto = CasosUsoProducto(repositorio_producto)
    caso_uso_obtener_carrito = CasoUsoObtenerCarrito(repositorio_carrito)
    caso_uso_agregar_al_carrito = CasoUsoAgregarAlCarrito(repositorio_carrito, repositorio_producto)
    caso_uso_eliminar_del_carrito = CasoUsoEliminarDelCarrito(repositorio_carrito)
    caso_uso_actualizar_item_carrito = CasoUsoActualizarItemCarrito(repositorio_carrito)
    
    # Inicializar CLI
    comandos = ComandosCLI(
        casos_uso_usuario, 
        casos_uso_producto, 
        caso_uso_obtener_carrito, 
        caso_uso_agregar_al_carrito, 
        caso_uso_eliminar_del_carrito, 
        caso_uso_actualizar_item_carrito
    )
    menu = MenuCLI(comandos)

    # Ejecutar la aplicación
    menu.ejecutar()

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

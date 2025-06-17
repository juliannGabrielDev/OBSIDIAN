---
aliases:
  - Pytest ✨
tags:
  - python
  - pruebas
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-python|🐍 Índice Python]]"
Fecha: 2025-06-02
---
# Pytest: Pruebas en Python más Fáciles ✨
- [[#1. ¿Qué es Pytest? 🤔|1. ¿Qué es Pytest? 🤔]]
- [[#2. ¿Por qué usar Pytest? (Ventajas) 👍|2. ¿Por qué usar Pytest? (Ventajas) 👍]]
- [[#3. Instalación 💻|3. Instalación 💻]]
- [[#4. Escribiendo Pruebas Básicas ✍️|4. Escribiendo Pruebas Básicas ✍️]]
- [[#5. Ejecutando Pruebas 🚀|5. Ejecutando Pruebas 🚀]]
- [[#6. Características Clave de Pytest 🔑|6. Características Clave de Pytest 🔑]]
- [[#7. Consejos y Buenas Prácticas 💡|7. Consejos y Buenas Prácticas 💡]]
- [[#Mini Repaso Final 📝|Mini Repaso Final 📝]]

---
## 1. ¿Qué es Pytest? 🤔
- **Pytest** es un **framework de pruebas (testing framework)** para Python.
- Te permite escribir desde pruebas unitarias pequeñas y simples hasta pruebas funcionales complejas para tus aplicaciones y bibliotecas.
- Se enfoca en hacer que escribir pruebas sea **fácil**, **rápido** y **menos verboso** en comparación con otros frameworks como `unittest` (que viene con Python).
- Es altamente **extensible** gracias a su sistema de plugins.
---
## 2. ¿Por qué usar Pytest? (Ventajas) 👍
Pytest tiene un montón de cosas chidas que lo hacen muy popular:
- **Sintaxis simple y concisa**: Escribir pruebas es muy intuitivo. No necesitas clases complejas para cada conjunto de pruebas si no quieres.
- **Detección automática de pruebas**: Pytest encuentra tus archivos de prueba y funciones de prueba automáticamente si sigues convenciones de nombres simples (archivos `test_*.py` o `*_test.py`, funciones `test_*`).
- **Afirmaciones (asserts) detalladas**: Cuando una prueba falla, Pytest te da información muy clara sobre qué valores no coincidieron, lo que facilita mucho la depuración.
- **Fixtures**: Un sistema poderoso y flexible para manejar el estado y las dependencias de tus pruebas (preparar datos, conexiones a bases de datos, etc.). ¡Son geniales!
- **Marcadores (Markers)**: Para categorizar pruebas, saltarlas condicionalmente, o marcarlas como "esperadas a fallar" (`xfail`).
- **Parametrización**: Ejecuta la misma prueba con diferentes conjuntos de datos de entrada/salida fácilmente.
- **Gran cantidad de plugins**: Para integración con otras herramientas (cobertura de código, Django, Flask, etc.) y para añadir funcionalidades.
- **Comunidad activa y buena documentación**.

---
## 3. Instalación 💻
Instalar Pytest es súper fácil usando `pip`:
```bash
pip install pytest
```
Y ¡listo! Ya lo puedes usar desde tu terminal.

---
## 4. Escribiendo Pruebas Básicas ✍️
- ### Convenciones de Nomenclatura:
    - Los archivos de prueba deben llamarse `test_*.py` o `*_test.py`.
        - Ejemplo: `test_mi_modulo.py`
    - Las funciones de prueba dentro de esos archivos deben empezar con `test_`.
        - Ejemplo: `def test_suma_positiva():`
    - Opcionalmente, puedes agrupar pruebas en clases que empiecen con `Test`. Dentro de estas clases, los métodos de prueba también deben empezar con `test_`.
        ```python
        # test_calculadora.py
        
        # Funciones de prueba simples
        def test_suma_numeros():
            assert 1 + 1 == 2
        
        def test_resta_numeros():
            assert 5 - 3 == 2
        
        # Pruebas agrupadas en una clase (opcional)
        class TestMultiplicacion:
            def test_multiplicar_por_cero(self):
                assert 10 * 0 == 0
        
            def test_multiplicar_positivos(self):
                assert 3 * 4 == 12
        ```
- ### Afirmaciones (Asserts):
    - Pytest usa la palabra clave estándar de Python **`assert`** para verificar si una condición es verdadera.
    - Si la condición después de `assert` es `False`, la prueba falla y Pytest muestra información útil.
    ```python
    # En un archivo como test_ejemplo.py
    
    def mi_funcion_a_probar(x, y):
        return x + y
    
    def test_mi_funcion_suma_correctamente():
        resultado = mi_funcion_a_probar(3, 5)
        assert resultado == 8  # Si resultado no es 8, la prueba falla
    
    def test_longitud_cadena():
        cadena = "hola"
        assert len(cadena) == 4
    ```
---
## 5. Ejecutando Pruebas 🚀
Para correr tus pruebas, simplemente navega en tu terminal a la carpeta raíz de tu proyecto (o donde estén tus archivos de prueba) y ejecuta:
```bash
pytest
```
- Pytest buscará recursivamente archivos y funciones que sigan las convenciones de nomenclatura y los ejecutará.
- Verás una salida que te indica cuántas pruebas pasaron (`.` o `PASSED`), cuántas fallaron (`F` o `FAILED`), cuántas se saltaron (`s` o `SKIPPED`), etc.
- **Opciones comunes:**
    - `pytest -v`: Modo verboso, muestra más detalles y el nombre de cada prueba.
    - `pytest -k "nombre_parcial"`: Ejecuta solo pruebas cuyos nombres contengan "nombre_parcial".
        - Ejemplo: `pytest -k "suma"` correría `test_suma_numeros` y `test_mi_funcion_suma_correctamente`.
    - `pytest nombre_del_archivo.py`: Ejecuta pruebas solo de un archivo específico.
    - `pytest nombre_del_archivo.py::nombre_de_la_funcion_test`: Ejecuta una función de prueba específica en un archivo.
    - `pytest --maxfail=N`: Detiene la ejecución después de N fallos.
    - `pytest -x`: Detiene la ejecución en el primer fallo.
---
## 6. Características Clave de Pytest 🔑
- ### Fixtures (Preparando el Terreno)
    - Las **fixtures** son funciones que Pytest ejecuta antes (y a veces después) de tus funciones de prueba. Sirven para configurar el entorno necesario para una prueba (ej. crear un objeto, una conexión a base de datos, datos de prueba).
    - Se definen con el decorador `@pytest.fixture`.
    - Luego, las funciones de prueba pueden "solicitar" una fixture simplemente incluyéndola como un argumento en su definición.
    ```python
    # test_fixtures.py
    import pytest
    
    @pytest.fixture
    def datos_iniciales():
        print("\n(Preparando datos iniciales...)") # Se ejecuta antes de cada prueba que la usa
        data = {"nombre": "Ejemplo", "valor": 10}
        yield data # 'yield' permite ejecutar código de limpieza después
        print("\n(Limpiando datos iniciales...)") # Se ejecuta después de la prueba
    
    def test_usando_fixture(datos_iniciales):
        assert datos_iniciales["nombre"] == "Ejemplo"
        assert datos_iniciales["valor"] > 5
    
    @pytest.fixture(scope="module") # 'scope' define con qué frecuencia se ejecuta la fixture
    def conexion_bd():
        print("\n(Conectando a la BD - una vez por módulo)")
        conn = "CONEXION_SIMULADA"
        yield conn
        print("\n(Cerrando conexión BD - una vez por módulo)")
    
    def test_operacion_bd_1(conexion_bd):
        assert conexion_bd == "CONEXION_SIMULADA"
    
    def test_operacion_bd_2(conexion_bd):
        assert conexion_bd is not None
    ```
    
    - **Scopes de Fixtures**: `function` (defecto, una vez por prueba), `class`, `module`, `package`, `session`.
- ### Marcadores (Markers - `@pytest.mark`)
    - Los marcadores te permiten añadir metadatos a tus pruebas. Pytest tiene algunos incorporados y puedes crear los tuyos.
    - Se usan como decoradores: `@pytest.mark.nombre_del_marcador`.
    - **`@pytest.mark.skip`**: Para saltar una prueba siempre.
        ```python
        @pytest.mark.skip(reason="Esta funcionalidad aún no está implementada.")
        def test_nueva_funcionalidad():
            assert False
        ```
        
    - **`@pytest.mark.skipif(condicion, reason="...")`**: Para saltar una prueba si se cumple una condición.
        ```python
        import sys
        @pytest.mark.skipif(sys.version_info < (3, 10), reason="Requiere Python 3.10+")
        def test_con_match_case():
            # Código que usa match-case
            pass
        ```
    - **`@pytest.mark.xfail`**: Indica que esperas que una prueba falle. Si falla, se marca como `XFAIL` (fallo esperado). Si inesperadamente pasa, se marca como `XPASS`.
        ```python
        @pytest.mark.xfail(reason="Bug conocido #123")
        def test_con_bug_conocido():
            assert 1 / 0 # Esto causará un error
        ```
    - **Marcadores personalizados**: Puedes definir tus propios marcadores en un archivo `pytest.ini` y luego usarlos para agrupar o seleccionar pruebas (`pytest -m "nombre_marcador"`).
        ```
        # pytest.ini
        [pytest]
        markers =
            slow: marca pruebas como lentas de ejecutar
            api: marca pruebas que interactúan con una API externa
        ```

        ```python
        # test_marcadores.py
        @pytest.mark.slow
        def test_proceso_largo():
            # ...
            pass
        
        @pytest.mark.api
        def test_llamada_a_api_externa():
            # ...
            pass
        ```
        Para correr solo las pruebas `slow`: `pytest -m slow`
- ### Parametrización (Pruebas con Múltiples Datos)
    - El decorador `@pytest.mark.parametrize("nombre_argumento", [valores])` te permite ejecutar la misma función de prueba múltiples veces con diferentes valores para los argumentos.
    ```python
    # test_parametrizacion.py
    import pytest
    
    def es_par(numero):
        return numero % 2 == 0
    
    @pytest.mark.parametrize("numero_test, resultado_esperado", [
        (2, True),
        (3, False),
        (0, True),
        (-4, True),
        (-5, False)
    ])
    def test_es_par_multiples_casos(numero_test, resultado_esperado):
        assert es_par(numero_test) == resultado_esperado
    ```
    Esta prueba se ejecutará 5 veces, una por cada tupla en la lista.
- ### Plugins y Extensibilidad
    - Pytest tiene un ecosistema de plugins muy rico. Algunos populares:
        - `pytest-cov`: Para medir la cobertura de código de tus pruebas.
        - `pytest-django`: Para probar aplicaciones Django.
        - `pytest-flask`: Para probar aplicaciones Flask.
        - `pytest-xdist`: Para ejecutar pruebas en paralelo, acelerando la ejecución.
        - `pytest-html`: Para generar reportes de prueba en formato HTML.
    - Puedes instalar plugins con `pip install nombre-del-plugin`.

---
## 7. Consejos y Buenas Prácticas 💡
- **Pruebas pequeñas y enfocadas**: Cada prueba debe verificar una sola cosa.
- **Nombres descriptivos**: Los nombres de tus funciones de prueba deben dejar claro qué están probando.
- **Independencia**: Las pruebas no deben depender del orden en que se ejecutan ni del resultado de otras pruebas. Usa fixtures para manejar dependencias.
- **Pruebas rápidas**: Especialmente las unitarias. Las pruebas lentas desaniman a ejecutarlas frecuentemente.
- **Prueba los casos límite y errores**: No solo el "camino feliz".
- **Integra las pruebas en tu flujo de trabajo**: Ejecútalas regularmente, idealmente con cada cambio (integración continua).
---
## Mini Repaso Final 📝
- **Pytest** es un framework para escribir pruebas en Python de forma **sencilla y potente**.
- Se instala con `pip install pytest` y se ejecuta con el comando `pytest`.
- Las pruebas son funciones que empiezan con `test_` en archivos `test_*.py` o `*_test.py`.
- Usa **`assert`** para verificar condiciones.
- Las **fixtures** (`@pytest.fixture`) son clave para preparar el entorno de prueba.
- Los **marcadores** (`@pytest.mark`) permiten categorizar, saltar o esperar fallos.
- La **parametrización** (`@pytest.mark.parametrize`) permite probar con múltiples datos fácilmente.
- ¡Es una herramienta muy útil para asegurar la calidad de tu código!
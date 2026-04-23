Este plan de trabajo está diseñado para transformar tus requisitos en un entorno de desarrollo profesional, utilizando una metodología de ingeniería de software basada en agentes y una estructura clara para estudiantes.

---

### 1. Estructura de Directorios (Workplace)
Ejecuta los siguientes comandos en tu terminal para preparar el entorno:

```bash
mkdir xflutterRios0676
cd xflutterRios0676
flutter create cruddogclub
cd cruddogclub
```

**Estructura sugerida:**
```text
lib/
├── models/
│   └── cuidador_model.dart
├── services/
│   └── crud_service.dart
├── screens/
│   └── home_screen.dart
└── main.dart
```

---

### 2. Metodología de Trabajo: Agentes y Roles
Para un entorno de aprendizaje (Guided Practice), dividiremos el proyecto en agentes especializados. Esto ayuda a los estudiantes a entender la separación de responsabilidades (SoC - Separation of Concerns).

| Agente / Rol | Responsabilidad (Skills) | Flujo de Trabajo |
| :--- | :--- | :--- |
| **Arquitecto de Datos** | Configuración de Firebase, modelado de esquemas. | Define `Cuidadores` con campos (Direccion, Nombre, Numero). |
| **Ingeniero Backend** | Lógica de negocio, integración Firestore. | Crea `crud_service.dart`. Gestiona conexión con consola. |
| **Diseñador UX/UI** | Aplicación de paleta "Antigravity", widgets. | Implementa `ThemeData` y componentes visuales. |
| **QA Specialist** | Pruebas de integración, verificación de CRUD. | Valida que los datos persistan correctamente. |

---

### 3. Configuración del Proyecto
#### Integración de Librerías
En tu archivo `pubspec.yaml`, agrega las dependencias bajo `dependencies:`:

```yaml
dependencies:
  flutter:
    sdk: flutter
  firebase_core: ^3.0.0
  cloud_firestore: ^5.0.0
```
Ejecuta `flutter pub get` en tu terminal.

#### Configuración Firebase
1. Ve a la [Consola de Firebase](https://console.firebase.google.com/).
2. Crea el proyecto: `DogClubCrud`.
3. Registra tu app (iOS/Android) y descarga el archivo `google-services.json` (Android) o `GoogleService-Info.plist` (iOS).
4. Colócalos en las carpetas `android/app/` y `ios/Runner/` respectivamente.

---

### 4. Implementación (Código Fuente)

#### Tema "Antigravity" (`main.dart`)
Configuramos la paleta de colores solicitada para dar una sensación de ligereza y modernidad.

```dart
import 'package:flutter/material.dart';
import 'package:firebase_core/firebase_core.dart';
import 'screens/home_screen.dart';

void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await Firebase.initializeApp();
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'DogClub App',
      theme: ThemeData(
        scaffoldBackgroundColor: const Color(0xFFF0F4F8), // Fondo
        appBarTheme: const AppBarTheme(backgroundColor: Color(0xFF1976D2)), // AppBar
        primaryColor: const Color(0xFF0D47A1), // Botones
        floatingActionButtonTheme: const FloatingActionButtonThemeData(
          backgroundColor: Color(0xFF0D47A1),
        ),
        colorScheme: ColorScheme.fromSwatch().copyWith(
          secondary: const Color(0xFFBBDEFB), // Acentos
        ),
      ),
      home: const HomeScreen(),
    );
  }
}
```

#### Modelo de Datos (`models/cuidador_model.dart`)
```dart
class Cuidador {
  String id;
  String nombre;
  String direccion;
  String numero;

  Cuidador({required this.id, required this.nombre, required this.direccion, required this.numero});

  Map<String, dynamic> toMap() => {'Nombre': nombre, 'Direccion': direccion, 'Numero': numero};
  
  factory Cuidador.fromMap(String id, Map<String, dynamic> map) {
    return Cuidador(id: id, nombre: map['Nombre'], direccion: map['Direccion'], numero: map['Numero']);
  }
}
```

#### Servicio CRUD (`services/crud_service.dart`)
```dart
import 'package:cloud_firestore/cloud_firestore.dart';
import '../models/cuidador_model.dart';

class CrudService {
  final CollectionReference _collection = FirebaseFirestore.instance.collection('Cuidadores');

  Future<void> addCuidador(Cuidador c) => _collection.add(c.toMap());
  
  Future<void> updateCuidador(Cuidador c) => _collection.doc(c.id).update(c.toMap());
  
  Future<void> deleteCuidador(String id) => _collection.doc(id).delete();

  Stream<List<Cuidador>> getCuidadores() {
    return _collection.snapshots().map((snapshot) => snapshot.docs
        .map((doc) => Cuidador.fromMap(doc.id, doc.data() as Map<String, dynamic>))
        .toList());
  }
}
```

#### Pantalla Principal (`screens/home_screen.dart`)
Aquí implementamos la interfaz funcional.

```dart
import 'package:flutter/material.dart';
import '../models/cuidador_model.dart';
import '../services/crud_service.dart';

class HomeScreen extends StatelessWidget {
  const HomeScreen({super.key});

  @override
  Widget build(BuildContext context) {
    final CrudService service = CrudService();

    return Scaffold(
      appBar: AppBar(title: const Text("DogClub Cuidadores")),
      body: StreamBuilder<List<Cuidador>>(
        stream: service.getCuidadores(),
        builder: (context, snapshot) {
          if (!snapshot.hasData) return const Center(child: CircularProgressIndicator());
          return ListView.builder(
            itemCount: snapshot.data!.length,
            itemBuilder: (context, index) {
              var item = snapshot.data![index];
              return ListTile(
                title: Text(item.nombre),
                subtitle: Text("${item.direccion} - ${item.numero}"),
                trailing: IconButton(
                  icon: const Icon(Icons.delete, color: Colors.red),
                  onPressed: () => service.deleteCuidador(item.id),
                ),
              );
            },
          );
        },
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: () => service.addCuidador(Cuidador(id: '', nombre: 'Nuevo Cuidador', direccion: 'Calle 123', numero: '555')),
        child: const Icon(Icons.add),
      ),
    );
  }
}
```

---

### Guía para la Práctica Estudiantil (Antigravity Style)
Para aplicar el enfoque "Antigravity", instruye a tus estudiantes a:
1. **Reducir la fricción visual:** Usar bordes redondeados (`RoundedRectangleBorder`) en todos los botones y cards (valor recomendado: 16.0).
2. **Jerarquía:** Asegurarse de que el color `0xFF0D47A1` siempre denote interacción (botones), mientras que el `0xFFBBDEFB` se use para fondos de cards o elementos de soporte.
3. **Validación:** Implementar validadores en los campos de texto para asegurar que `Numero` solo contenga dígitos.

¿Te gustaría que profundizara en la creación de los formularios de edición con validación de datos para completar el ciclo CRUD?

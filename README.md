# phoenix
Access
# 📂 Phoenix Android App – Estructura del Proyecto

Este documento describe la estructura general del proyecto Android **Phoenix**, siguiendo principios de Clean Architecture y organización modular. La estructura incluye distinción clara entre capas de dominio, datos, presentación y aplicación.

---

```
phoenix/                             # Proyecto principal de la app
├── application/                     # Coordinación de lógica de negocio y casos de uso
│   ├── communication/              # Orquestador del entorno general de comunicación
│   │   └── EnvironmentOrchestrator.kt  # Gestiona configuración de entorno y coordinación de servicios
│   ├── handlers/                   # Manejadores de mensajes entrantes desde UM
│   │   ├── BaseMessageHandler.kt       # Clase base para todos los handlers de mensajes
│   │   ├── BusinessInfoHandler.kt     # Maneja mensajes de información de negocio desde UM
│   │   ├── ConnectHandler.kt          # Maneja solicitudes de conexión desde UM
│   │   ├── LoginHandler.kt            # Maneja flujo de login con UM
│   │   ├── PasoInfoHandler.kt         # Maneja mensajes de paso de información (PasoInfo)
│   │   ├── PingHandler.kt             # Maneja mensajes de ping para control de latencia/vida
│   │   ├── SmartEnvInfoHandler.kt     # Maneja info del entorno inteligente
│   │   └── WhoAreYouHandler.kt        # Maneja identificación de dispositivos
│   └── useCase/                   # Casos de uso del dominio que ejecuta la lógica de negocio
│       └── PinPassLoginUseCase.kt    # Lógica del inicio de sesión con pin/pass
│    ag └── 🔷 SipCallUseCase.kt         # [SIP/DTMF] Caso de uso principal para llamadas SIP y envío DTMF
│
├── core/                          # Recursos centrales reutilizables
│   ├── extensions/                # Métodos de extensión para distintas clases
│   │   └── ImageExtensions.kt         # Funciones para manipular imágenes
│   └── 🔷Configuracion.kt           # Configuraciones generales de la app
│
├── data/                          # Capa de acceso a datos
│   ├── api/                       # Llamadas a APIs externas (ej. reconocimiento facial)
│   │   ├── IFaceRecognitionApi.kt     # Interfaz para API de reconocimiento facial
│   │   └── dto/                      # DTOs para la API
│   │       └── FaceRecognitionDtos.kt # Modelos de datos para la API
│   ├── di/                        # Inyección de dependencias
│   │   ├── AppBindsModule.kt
│   │   ├── AppModule.kt
│   │   ├── NetworkModule.kt
│   │   └── UMBindsModule.kt
│   ├── mock/                     # Implementaciones mock para pruebas
│   │   ├── MockedLoginService.kt
│   │   └── MockedNavigationBarService.kt
│   ├── service/                  # Servicios concretos de la app
│   │   ├── 🔷sipCalls/                 # [SIP/DTMF] Servicios para llamadas SIP/DTMF
│   │   │   🔷 sipCalls/                 # [SIP/DTMF] Servicios relacionados a llamadas SIP/DTMF
│   │   │   ├── 🔷 LinphoneSipService.kt     # [SIP/DTMF] Implementación concreta de llamadas SIP con Linphone
│   │   │   ├── 🔷 SimpleCoreListener.kt    # [SIP/DTMF] Escucha eventos de llamadas SIP y delega a lógica
│   │   │ag └── 🔷 DtmfToneSender.kt        # [SIP/DTMF] Encapsula el envío de tonos DTMF a través de Linphone
│   │   ├── um/                      # Comunicación con módulos UM
│   │   │   ├── dto/                    # Modelos de datos UM
│   │   │   │   ├── BusinessInfo.kt
│   │   │   │   ├── PasoInfo.kt
│   │   │   │   ├── SmartEnvInfo.kt
│   │   │   │   └── UMEnvironment.kt
│   │   │   └── transport/             # Transporte TCP/WebSocket
│   │   │       ├── TcpTransport.kt
│   │   │       └── WebSocketTransport.kt
│   │   ├── AwsReknitionService.kt
│   │   ├── BlinkDetectionService.kt
│   │   ├── Camara2FaceDetector.kt
│   │   ├── CamaraResourcesService.kt
│   │   ├── InactivityService.kt
│   │   ├── JsonSerializer.kt
│   │   ├── LivenessDetectionService.kt
│   │   ├── LocalAppVersionService.kt
│   │   ├── LocalFileLogger.kt
│   │   ├── MLKitFaceDetectionService.kt
│   │   └── RakindaNavigationBarService.kt
│
├── domain/                       # Lógica de negocio pura y contratos
│   ├── model/                    # Entidades de negocio
│   │   ├── communication/           # Modelos de mensajes UM
│   │   │   ├── JsonMessageKeys.kt
│   │   │   ├── MessageTypes.kt
│   │   │   └── login/
│   │   │       ├── LoginParams.kt
│   │   │       ├── LoginProtocol.kt
│   │   │       ├── LoginResponseAction.kt
│   │   │       ├── LoginResponseState.kt
│   │   │       ├── LoginResult.kt
│   │   │       └── LoginType.kt
│   │   ├── face/                    # Modelos de reconocimiento facial
│   │   │   ├── DomainFace.kt
│   │   │   ├── RecognizedFace.kt
│   │   │   └── RecognizedFaceResultCode.kt
│   │   ├── hardware/                # Representaciones de hardware
│   │   │   └── Cg800Camara.kt
│   │   ├── ui/                      # Estados visuales/UI
│   │   │   ├── ButtonData.kt
│   │   │   ├── NavigationBarVisibility.kt
│   │   │   └── 🔷SipUiState.kt           # [SIP/DTMF] Estado de UI de llamadas
│   │   └── um/                      # Modelos protocolo UM
│   │       ├── BaseMessage.kt
│   │       ├── request/
│   │       └── response/              # (login, ping, paso, etc.)
│   └── service/                  # Interfaces de servicios usados
│       ├── communication/           # 🔷[SIP/DTMF] Contratos de comunicación
│       │   ├── ICommunicationManager.kt
│       │   ├── IMessageDispatcher.kt
│       │   ├── IMessageHandler.kt
│       │   └── ITransport.kt
│       ├── face/                    # Interfaces de detección facial
│       │   ├── IFaceDetectionService.kt
│       │   ├── ILiveFaceDetection.kt
│       │   └── ILivenessDetectionService.kt
│       ├── IAppVersionService.kt
│       ├── ICameraResources.kt
│       ├── IConfiguration.kt
│       ├── IEnvironmentService.kt
│       ├── IInactivityService.kt
│       ├── ILogger.kt
│       ├── ILoginService.kt
│       ├── IMessageSerializer.kt
│       ├── INavigationBarService.kt
│       ├── IPinPassLoginUseCase.kt
│       ├── IRecognizeFaceService.kt
│       ├── 🔷ISipService.kt             # [SIP/DTMF] Contrato de llamadas SIP
│   ag  ├── 🔷 IDtmfSender.kt             # [SIP/DTMF] Contrato para el envío de tonos DTMF
│       └── IUseCase.kt
│
├── presentation/                 # Capa de presentación: UI y navegación
│   ├── components/               # Componentes reutilizables
│   │   ├── CameraFaceDetector.kt
│   │   ├── CameraPreview.kt
│   │   ├── 🔷DtmfToast.kt              # [SIP/DTMF] Notificación visual DTMF
│   │   ├── LongPressImage.kt
│   │   ├── NavBar.kt
│   │   ├── VersionText.kt
│   │   └── VideoPlayer.kt
│   ├── 🔷navigation/               # Flujo de navegación
│   │   ├── AppNavHost.kt
│   │   └── AppRoutes.kt
│   ├── theme/                    # Tema visual
│   ├── viewmodels/              # Controladores de estado de pantallas
│   │   ├── FaceLoginViewModel.kt
│   │   ├── FailureViewModel.kt
│   │   ├── HomeViewModel.kt
│   │   ├── PinViewModel.kt
│   │   ├── 🔷SipCallingViewModel.kt    # [SIP/DTMF] Lógica de llamadas y tonos
│   │   ├── SuccessViewModel.kt
│   │   └── TechnicalViewModel.kt
│   ├── views/                    # Pantallas visibles
│   │   ├── FaceLoginScreen.kt
│   │   ├── FailureScreen.kt
│   │   ├── HomeScreen.kt
│   │   ├── PinScreen.kt
│   │   ├── 🔷SelectionScreen.kt
│   │   ├── 🔷SipCallingScreen.kt      # [SIP/DTMF] UI de llamadas SIP
│   │   ├── SplashScreen.kt
│   │   ├── SuccessScreen.kt
│   │   └── TechnicalScreen.kt
│   └──MainActivity
└───PhoenixApp
```

---

✅ **Notas finales**
- Las secciones destacadas como `[SIP/DTMF]` están ligadas a funcionalidades específicas de llamadas.



```
🟩 phoenix/
Proyecto principal que agrupa todos los módulos de la app.
🟧 application/
Coordina la lógica de negocio y ejecución de casos de uso.
communication/
EnvironmentOrchestrator.kt: Orquesta y configura el entorno de ejecución general (red, servicios, etc.).

handlers/
BaseMessageHandler.kt: Clase base para manejar mensajes entrantes del módulo UM.

BusinessInfoHandler.kt: Procesa información de negocio enviada por UM.

ConnectHandler.kt: Maneja solicitudes de conexión del sistema.

LoginHandler.kt: Coordina el inicio de sesión a través del módulo UM.

PasoInfoHandler.kt: Procesa información de pasos o secuencias del sistema.

PingHandler.kt: Responde a pings para mantener la conexión viva.

SmartEnvInfoHandler.kt: Recibe y gestiona información de entorno inteligente.

WhoAreYouHandler.kt: Identifica al dispositivo que se conecta al sistema.

useCase/
PinPassLoginUseCase.kt: Lógica de autenticación con PIN o contraseña.

SipCallUseCase.kt: Lógica principal para iniciar llamadas SIP y enviar tonos DTMF.

🟧 core/
Utilidades y configuraciones reutilizables.
extensions/ImageExtensions.kt: Funciones para editar o manipular imágenes.

Configuracion.kt: Configuración central de parámetros generales de la app.

🟧 data/
Acceso a datos y servicios concretos.
api/
IFaceRecognitionApi.kt: Interfaz para el API de reconocimiento facial.

dto/FaceRecognitionDtos.kt: Clases para enviar/recibir datos del API.

di/
AppBindsModule.kt: Módulo de bindings de servicios generales.

AppModule.kt: Provee instancias generales (contexto, servicios, etc.).

NetworkModule.kt: Configura networking (retrofit, okhttp, etc.).

UMBindsModule.kt: Inyecciones específicas para módulo UM.

mock/
MockedLoginService.kt: Simula inicio de sesión para pruebas.

MockedNavigationBarService.kt: Mock de la barra de navegación.

service/
sipCalls/
LinphoneSipService.kt: Lógica concreta de llamadas SIP con Linphone.

SimpleCoreListener.kt: Escucha eventos de llamadas SIP y los reenvía.

DtmfToneSender.kt: Encapsula el envío de tonos DTMF por SIP.

um/dto/
BusinessInfo.kt, PasoInfo.kt, SmartEnvInfo.kt, UMEnvironment.kt: Modelos de datos utilizados en mensajes UM.

um/transport/
TcpTransport.kt: Envío de mensajes vía TCP.

WebSocketTransport.kt: Comunicación vía WebSocket.

Servicios generales:
AwsReknitionService.kt: Usa AWS Rekognition para análisis facial.

BlinkDetectionService.kt: Detecta parpadeos para validación facial.

Camara2FaceDetector.kt: Implementación de detección facial con Camera2.

CamaraResourcesService.kt: Administra recursos de cámara.

InactivityService.kt: Controla inactividad del usuario.

JsonSerializer.kt: Serialización/deserialización de mensajes JSON.

LivenessDetectionService.kt: Verifica si el rostro es real (no foto).

LocalAppVersionService.kt: Obtiene versión local de la app.

LocalFileLogger.kt: Guarda logs en archivos locales.

MLKitFaceDetectionService.kt: Detección facial usando MLKit.

RakindaNavigationBarService.kt: Control de barra de navegación Rakinda.

🟧 domain/
Lógica de negocio pura, entidades y contratos.
model/communication/
JsonMessageKeys.kt: Constantes de claves en mensajes JSON.

MessageTypes.kt: Tipos de mensajes posibles.

login/:

LoginParams.kt: Parámetros para login.

LoginProtocol.kt: Protocolo para autenticación.

LoginResponseAction.kt: Posibles acciones post-login.

LoginResponseState.kt: Estado tras la respuesta de login.

LoginResult.kt: Resultado de autenticación.

LoginType.kt: Tipos de login disponibles.

model/face/
DomainFace.kt: Representación facial general.

RecognizedFace.kt: Rostro reconocido.

RecognizedFaceResultCode.kt: Códigos de resultado de reconocimiento.

model/hardware/
Cg800Camara.kt: Representación de cámara CG800.

model/ui/
ButtonData.kt: Información para botones UI.

NavigationBarVisibility.kt: Estado de visibilidad de la navbar.

SipUiState.kt: Estado de la UI para llamadas SIP y DTMF.

model/um/
BaseMessage.kt: Mensaje base para UM.

request/ y response/: Estructura de mensajes entrantes/salientes del protocolo UM.

service/
Comunicación (SIP/UM):
ICommunicationManager.kt: Coordinador de comunicación general.

IMessageDispatcher.kt: Encargado de enviar mensajes.

IMessageHandler.kt: Maneja mensajes recibidos.

ITransport.kt: Medio de transporte (TCP, WS).

Reconocimiento facial:
IFaceDetectionService.kt: Servicio de detección facial.

ILiveFaceDetection.kt: Servicio de detección en tiempo real.

ILivenessDetectionService.kt: Verifica vitalidad facial.

Generales:
IAppVersionService.kt: Acceso a la versión de la app.

ICameraResources.kt: Controla recursos de cámara.

IConfiguration.kt: Provee configuración general.

IEnvironmentService.kt: Información del entorno actual.

IInactivityService.kt: Provee servicios de inactividad.

ILogger.kt: Contrato de logging.

ILoginService.kt: Servicio de login.

IMessageSerializer.kt: Serializa mensajes.

INavigationBarService.kt: Controla visibilidad y estados de la navbar.

IPinPassLoginUseCase.kt: Caso de uso de login.

IRecognizeFaceService.kt: Servicio para reconocer rostro.

ISipService.kt: Contrato de llamadas SIP.

IDtmfSender.kt: Contrato de envío de DTMF.

IUseCase.kt: Contrato base de cualquier caso de uso.

🟧 presentation/
Capa de presentación y UI.
components/
CameraFaceDetector.kt: Vista que detecta caras usando la cámara.

CameraPreview.kt: Vista previa de la cámara.

DtmfToast.kt: Muestra un toast visual al enviar DTMF.

LongPressImage.kt: Imagen con acción de mantener pulsado.

NavBar.kt: Barra de navegación inferior.

VersionText.kt: Muestra la versión actual de la app.

VideoPlayer.kt: Reproductor de video en pantalla.

navigation/
AppNavHost.kt: Host de navegación general.

AppRoutes.kt: Rutas disponibles de navegación.

theme/
Configuración visual: colores, tipografías, tamaños (no detallado aquí).

viewmodels/
FaceLoginViewModel.kt: Estado de UI para login con rostro.

FailureViewModel.kt: Manejo de errores.

HomeViewModel.kt: Estado y acciones desde el home.

PinViewModel.kt: Estado para login con PIN.

SipCallingViewModel.kt: Controla llamadas SIP y tonos DTMF.

SuccessViewModel.kt: Vista exitosa.

TechnicalViewModel.kt: Vista de información técnica.

views/
FaceLoginScreen.kt: Pantalla de login facial.

FailureScreen.kt: Pantalla de error.

HomeScreen.kt: Pantalla principal.

PinScreen.kt: Pantalla de login con PIN.

SelectionScreen.kt: Pantalla para elegir modo de ingreso.

SipCallingScreen.kt: Pantalla principal de llamadas SIP.

SplashScreen.kt: Pantalla inicial.

SuccessScreen.kt: Pantalla de confirmación exitosa.

TechnicalScreen.kt: Vista con detalles técnicos.

MainActivity
Actividad raíz que lanza toda la app.

🟩 PhoenixApp
Punto de entrada @HiltAndroidApp, inicializa todo al iniciar la app.
```


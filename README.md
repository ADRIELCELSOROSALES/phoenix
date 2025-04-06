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
```

---

✅ **Notas finales**
- Las secciones destacadas como `[SIP/DTMF]` están ligadas a funcionalidades específicas de llamadas.
- La estructura promueve separación de responsabilidades, reutilización y testeo.
- Este README puede ser actualizado conforme evolucione el proyecto.

---

¿Te gustaría que agregue una breve introducción del proyecto o guía para nuevos desarrolladores?

Diagrama

.
├── docker-compose.yml
├── .env
├── services/
│   ├── auth-service/
│   │   ├── src/
│   │   │   ├── app.js
│   │   │   ├── config/
│   │   │   │   └── db.js
│   │   │   ├── controllers/
│   │   │   │   └── authController.js
│   │   │   ├── models/
│   │   │   │   └── User.js
│   │   │   └── routes/
│   │   │       └── authRoutes.js
│   │   ├── Dockerfile
│   │   └── package.json
│   │
│   ├── catalog-service/
│   │   ├── src/
│   │   │   ├── app.js
│   │   │   ├── config/
│   │   │   │   └── db.js
│   │   │   ├── controllers/
│   │   │   │   └── catalogController.js
│   │   │   ├── models/
│   │   │   │   └── Movie.js
│   │   │   └── routes/
│   │   │       └── catalogRoutes.js
│   │   ├── Dockerfile
│   │   └── package.json
│   │
│   ├── processing-service/  # NOVO: Esqueleto do Serviço de Processamento
│   │   ├── src/
│   │   │   ├── app.js
│   │   │   ├── config/
│   │   │   │   └── rabbitmq.js # Configuração do RabbitMQ específica para este serviço
│   │   │   ├── listeners/
│   │   │   │   └── videoUploadListener.js
│   │   │   └── controllers/
│   │   │       └── processingController.js # Endpoint de teste para simular upload
│   │   ├── Dockerfile
│   │   └── package.json
│   │
│   └── shared/                # NOVO: Contratos de eventos (centralizados)
│       └── events/
│           ├── index.js       # Exporta todos os schemas
│           ├── userEvents.js  # Schema para eventos de usuário
│           └── videoEvents.js # Schema para eventos de vídeo
│
└── infrastructure/
    ├── rabbitmq/
    ├── postgres/
    └── nginx/

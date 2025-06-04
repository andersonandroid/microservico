graph TD
    %% Usuário e Plataformas
    User[Usuário (Smart TVs, Mobile, Web)] --> |Requisição HTTP/HTTPS| APIGateway(API Gateway)

    %% Microsserviços Principais
    subgraph Microsserviços Core
        APIGateway --> |API REST/gRPC| CatalogoDescobertaService[Serviço de Catálogo e Descoberta]
        APIGateway --> |API REST/gRPC| StreamingService[Serviço de Streaming]
        APIGateway --> |API REST| AuthService[Serviço de Autenticação e Perfil]
        APIGateway --> |API REST| PagamentosService[Serviço de Pagamentos]
        APIGateway --> |API REST| UploadService[Serviço de Upload]

        CatalogoDescobertaService --> DB_Catalogo[DB Catálogo (Elasticsearch/NoSQL/Cache)]
        StreamingService --> DB_Visualizacao[DB Visualização (NoSQL/Time-series)]
        AuthService --> DB_Usuarios[DB Usuários (Relacional)]
        PagamentosService --> DB_Pagamentos[DB Pagamentos (Relacional/NoSQL)]

        UploadService --> ProcessamentoConteudoService[Serviço de Processamento de Conteúdo]
        ProcessamentoConteudoService --> DB_Processamento[DB Processamento (NoSQL/Filas)]
        ProcessamentoConteudoService --> CDN(CDN Global)

        %% Comunicação Assíncrona via Fila de Mensagens (Kafka/RabbitMQ)
        UploadService --> |Evento: 'VideoUploadIniciado' (Fila)| MessageBroker(Fila de Mensagens / Broker)
        ProcessamentoConteudoService --> |Evento: 'VideoProcessadoEModerado' (Fila)| MessageBroker
        StreamingService --> |Evento: 'StreamIniciado' (Fila)| MessageBroker
        StreamingService --> |Evento: 'ProgressoDeVisualizacao' (Fila)| MessageBroker
        AuthService --> |Evento: 'UsuarioLogado/Atualizado' (Fila)| MessageBroker

        MessageBroker --> CatalogoDescobertaService
        MessageBroker --> AnalyticsService[Serviço de Analytics]
        MessageBroker --> RecomendacaoService[Serviço de Recomendação]
        MessageBroker --> NotificacoesService[Serviço de Notificações]
        MessageBroker --> ModeraçãoService[Serviço de Moderação]

        CatalogoDescobertaService --> RecomendacaoService
        RecomendacaoService --> DB_Recomendacao[DB Recomendação (GraphDB/VectorDB)]
        AnalyticsService --> DB_Analytics[DB Analytics (Data Lake/DW)]

        %% Integrações Externas
        ProcessamentoConteudoService --> |Armazenamento de Vídeos| CDN
        StreamingService --> |Entrega de Conteúdo| CDN
        RecomendacaoService --> |API ML Externo| MLServices(Serviços de ML Externos)
        PagamentosService --> |API Externa| GatewayPagamento(Gateway de Pagamento)
        AuthService --> |API Externa| SocialMedia(Redes Sociais p/ Login)
        ModeraçãoService --> |API Externa| DRMServices(Sistemas de DRM)
        AnalyticsService --> |Webhook/Batch| ExternalAnalytics(Analytics Externos)

    end

    subgraph Componentes de Infraestrutura
        ServiceRegistry(Registro de Serviços)
        ConfigService(Serviço de Configuração)
        MonitoringLogging[Monitoramento & Logs (Prometheus, Grafana, ELK)]
        LoadBalancer(Load Balancer)
        CacheDistribuido(Cache Distribuído - Redis/Memcached)
    end

    APIGateway -- Gerencia --> LoadBalancer
    LoadBalancer -- Rota para --> CatalogoDescobertaService
    LoadBalancer -- Rota para --> StreamingService
    LoadBalancer -- Rota para --> AuthService
    LoadBalancer -- Rota para --> PagamentosService
    LoadBalancer -- Rota para --> UploadService
    LoadBalancer -- Rota para --> ProcessamentoConteudoService


    APIGateway --> ServiceRegistry
    CatalogoDescobertaService --> ServiceRegistry
    StreamingService --> ServiceRegistry
    AuthService --> ServiceRegistry
    PagamentosService --> ServiceRegistry
    UploadService --> ServiceRegistry
    ProcessamentoConteudoService --> ServiceRegistry
    AnalyticsService --> ServiceRegistry
    RecomendacaoService --> ServiceRegistry
    NotificacoesService --> ServiceRegistry
    ModeraçãoService --> ServiceRegistry


    APIGateway --> ConfigService
    CatalogoDescobertaService --> ConfigService
    StreamingService --> ConfigService
    AuthService --> ConfigService
    PagamentosService --> ConfigService
    UploadService --> ConfigService
    ProcessamentoConteudoService --> ConfigService
    AnalyticsService --> ConfigService
    RecomendacaoService --> ConfigService
    NotificacoesService --> ConfigService
    ModeraçãoService --> ConfigService


    APIGateway --> MonitoringLogging
    CatalogoDescobertaService --> MonitoringLogging
    StreamingService --> MonitoringLogging
    AuthService --> MonitoringLogging
    PagamentosService --> MonitoringLogging
    UploadService --> MonitoringLogging
    ProcessamentoConteudoService --> MonitoringLogging
    AnalyticsService --> MonitoringLogging
    RecomendacaoService --> MonitoringLogging
    NotificacoesService --> MonitoringLogging
    ModeraçãoService --> MonitoringLogging

    CatalogoDescobertaService --> CacheDistribuido
    StreamingService --> CacheDistribuido
    RecomendacaoService --> CacheDistribuido

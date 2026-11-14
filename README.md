# 🍔 Case Study Arquitetural — EstouOn Food (SaaS Multi-Restaurante & IA Multimodal)

![Status](https://img.shields.io/badge/Status-Em_Produção-10B981?style=for-the-badge)
[![Produção](https://img.shields.io/badge/Acessar-estouon.app.br-EF4444?style=for-the-badge)](https://estouon.app.br)
[![Instagram Oficial](https://img.shields.io/badge/Instagram-%40estouon.app-E4405F?style=for-the-badge&logo=instagram&logoColor=white)](https://www.instagram.com/estouon.app/)
![Arquitetura](https://img.shields.io/badge/Arquitetura-PostGIS_%2B_Typesense_%2B_Reverb-3B82F6?style=for-the-badge)

> **Nota de Governança:** Este repositório público documenta a arquitetura de engenharia do **EstouOn Food** (`estouon.app.br`). O código-fonte de produção é mantido em repositório privado.

---

## 🎯 Sobre a Plataforma

O **EstouOn Food** é uma plataforma SaaS Multi-Restaurante, Delivery e Cardápio Digital construída para eliminar taxas abusivas de intermediação sobre comércios locais (restaurantes, hamburguerias, food trucks, mercados e hortifrutis).

A plataforma combina uma arquitetura de **alta conversão e baixa latência** com um **Esquadrão Autônomo de Marketing e Atendimento por IA (`Luís IA`)** que produz campanhas multimodais (Reels em vídeo, carrosséis e locuções) e responde clientes em tempo real no Instagram e WhatsApp.

---

## 🏗️ Arquitetura de Produção & Diferenciais Técnicos

```mermaid
flowchart TB
    subgraph Client["Experiência do Consumidor & Lojista"]
        PWA["Vitrine PWA de Alta Conversão (React / TypeScript)"]
        MERCH["Painel do Lojista & KDS em Tempo Real"]
    end

    subgraph Backend["Núcleo EstouOn Food (Docker Swarm)"]
        API["Laravel API + Workers de Fila + Scheduler"]
        REV["Laravel Reverb (WebSockets Tempo Real)"]
        PG["PostgreSQL + PostGIS (Raios de Entrega Geoespaciais)"]
        TS["Typesense (Busca Instantânea Tolerante a Erros)"]
        RD["Redis (Filas, Cache & Sessões)"]
    end

    subgraph AISquad["Esquadrão Autônomo de IA (Luís IA)"]
        LUIS["Orquestrador Luís (Google ADK + LangGraph)"]
        STUDIO["Estúdio Multimodal (Veo 3.1 Vídeo + ElevenLabs/Chirp3-HD + Lyria-002 -14 LUFS)"]
        META["Meta Graph API v27 (Reels Resumable Upload + Webhooks de Comentários/DMs)"]
    end

    PWA & MERCH --> API
    PWA & MERCH <--> REV
    API --> PG & TS & RD
    LUIS --> STUDIO --> META
```

### Engenharia de Destaque
- **Cálculo Geoespacial com `PostGIS`:** Validação precisa de polígonos e raios de entrega com indexação espacial para cálculo instantâneo de frete.
- **Busca Instantânea com `Typesense`:** Indexação de cardápios e catálogos multi-loja com resposta sub-50ms.
- **Operação em Tempo Real (`Laravel Reverb`):** Atualização instantânea de status de pedidos da cozinha até o cliente final sem *polling* HTTP.
- **Marketing & Atendimento Autônomo (`Luís IA`):** Agente de domínio dedicado com personagem 3D próprio, direção criativa multimodal segmentada por nicho, armazenamento em **Cloudflare R2 (`S3v4`)** e *LLM-as-a-Judge* para conformidade publicitária.

---

## 🛠️ Stack Tecnológica

![PHP](https://img.shields.io/badge/PHP_8.4-777BB4?style=for-the-badge&logo=php&logoColor=white)
![Laravel](https://img.shields.io/badge/Laravel-FF2D20?style=for-the-badge&logo=laravel&logoColor=white)
![React](https://img.shields.io/badge/React-61DAFB?style=for-the-badge&logo=react&logoColor=black)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white)
![PostgreSQL PostGIS](https://img.shields.io/badge/PostGIS-4169E1?style=for-the-badge&logo=postgresql&logoColor=white)
![Typesense](https://img.shields.io/badge/Typesense-D92773?style=for-the-badge&logo=algolia&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?style=for-the-badge&logo=redis&logoColor=white)
![Docker Swarm](https://img.shields.io/badge/Docker_Swarm-2496ED?style=for-the-badge&logo=docker&logoColor=white)

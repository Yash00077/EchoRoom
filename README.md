# Real-Time Chat Application

A full-stack real-time chat platform composed of a Next.js frontend and three microservices (User, Chat, Mail), backed by MongoDB, Redis, RabbitMQ, and Socket.IO.

## Architecture

```
chat code/
├── frontend/          # Next.js 15 (React 19) web client
└── backend/
    ├── user/          # Auth, users, Redis token cache, RabbitMQ producer
    ├── chat/          # Chat rooms, messages, Socket.IO, Cloudinary uploads
    └── mail/          # Email consumer (RabbitMQ -> Nodemailer)
```

## Tech Stack

- **Frontend:** Next.js 15, React 19, TypeScript, Tailwind CSS 4, Socket.IO client, Axios
- **Backend services:** Node.js, Express 5, TypeScript
- **Data:** MongoDB (Mongoose), Redis (Upstash)
- **Messaging:** RabbitMQ (amqplib)
- **Realtime:** Socket.IO
- **Media:** Cloudinary
- **Email:** Nodemailer (Gmail SMTP)

## Prerequisites

- Node.js 18+
- Docker (for RabbitMQ) or a running RabbitMQ instance
- MongoDB Atlas (or local MongoDB)
- Redis instance (Upstash or local)
- Cloudinary account
- Gmail app password (for mail service)

## Environment Variables

Create `.env` in each backend service (templates exist).

**backend/user/.env**
```
MONGO_URI=<mongodb-uri>
PORT=5000
REDIS_URL=<redis-url>
Rabbitmq_Host=localhost
Rabbitmq_Username=admin
Rabbitmq_Password=admin123
JWT_SECRET=<secret>
```

**backend/chat/.env**
```
PORT=5002
MONGO_URI=<mongodb-uri>
JWT_SECRET=<secret>
USER_SERVICE=http://localhost:5000
Cloud_Name=<cloudinary-cloud>
Api_Key=<cloudinary-key>
Api_Secret=<cloudinary-secret>
```

**backend/mail/.env**
```
PORT=5001
Rabbitmq_Host=localhost
Rabbitmq_Username=admin
Rabbitmq_Password=admin123
USER=<gmail-address>
PASSWORD=<gmail-app-password>
```

## Setup

### 1. Start RabbitMQ
```bash
docker run -d --name rabbitmq -p 5672:5672 -p 15672:15672 \
  -e RABBITMQ_DEFAULT_USER=admin -e RABBITMQ_DEFAULT_PASS=admin123 \
  rabbitmq:3-management
```

### 2. Install dependencies
```bash
cd backend/user && npm install
cd ../chat && npm install
cd ../mail && npm install
cd ../../frontend && npm install
```

### 3. Build TypeScript backends
```bash
cd backend/user && npm run build
cd ../chat && npm run build
cd ../mail && npm run build
```

### 4. Run services
```bash
# Terminal 1
cd backend/user && npm start
# Terminal 2
cd backend/chat && npm start
# Terminal 3
cd backend/mail && npm start
# Terminal 4
cd frontend && npm run dev
```

Or use dev mode with hot reload (`npm run dev` in each backend).

## Ports

| Service   | Port |
|-----------|------|
| Frontend  | 3000 |
| User      | 5000 |
| Mail      | 5001 |
| Chat      | 5002 |
| RabbitMQ  | 5672 / 15672 |

## Features

- User registration & login (JWT)
- Real-time messaging via Socket.IO
- Chat rooms / conversations
- Media uploads (Cloudinary)
- Email notifications via RabbitMQ consumer
- Redis-backed token/session cache

## Development Notes

- Frontend uses Next.js App Router (`src/app`).
- Backends are ESM (`"type": "module"`); compiled output in `dist/`.
- RabbitMQ management UI: http://localhost:15672 (admin/admin123).

## License

MIT

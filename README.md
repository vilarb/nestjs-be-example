# NESTJS BACKEND EXAMPLE APP

Hi. This is the begging stage of a backend I had to build for a project and server simply as an example. I have added some caveats that could be important and clarify some things regarding my decision making at the end. The original intention behind this dasboard was user acquisition event tracking as can be seen from the code structure. The project is built with [NestJs](https://nestjs.com) which is very similar to the more popular expressJs, but more opinionated. At the time I chose it for its structure and strong typing which I still appreciate.

## 🚀 Features

- **Event Management**: Full CRUD operations for events with priority levels and types
- **User Authorization**: IP-based authorization system for ads-type events
- **Database Integration**: PostgreSQL with TypeORM for data persistence
- **API Documentation**: Swagger/OpenAPI documentation
- **Docker Support**: Complete containerization with Docker Compose
- **Testing**: Unit and E2E tests with Jest
- **Code Quality**: ESLint and Prettier configuration

## 📋 Prerequisites

- Node.js 20+
- Docker and Docker Compose
- PostgreSQL (if running locally)

## 🏗️ Architecture

The project follows a modular NestJS architecture with:

- **Controllers**: Handle HTTP requests and responses
- **Services**: Business logic implementation
- **Entities**: TypeORM database models
- **DTOs**: Data Transfer Objects for validation
- **Guards**: Request authorization and validation
- **Modules**: Feature-based organization

## 📁 Project Structure

```
src/
├── app.controller.ts          # Main application controller
├── app.module.ts             # Root module
├── app.service.ts            # Main application service
├── main.ts                   # Application entry point
├── events/                   # Events module
│   ├── controllers/          # Event controllers
│   ├── services/            # Event services
│   ├── entities/            # Event entities
│   ├── dto/                 # Event DTOs
│   └── guards/              # Event guards
└── users/                   # Users module
    ├── controllers/         # User controllers
    ├── services/           # User services
    └── interfaces/         # User interfaces
```

## 🛠️ Installation & Setup

1. **Clone the repository**

   ```bash
   git clone git@github.com:vilarb/nestjs-be-example.git
   cd nestjs-be-example
   ```

2. **Create environment file and add the correct values**

   ```bash
   cp .env.example .env
   ```

3. **Start the application**
   ```bash
   docker-compose up --build
   ```

The API will be available at `http://localhost:3000`

## 🧪 Testing

### Run Tests

```bash
# Unit tests
npm run test

# Unit tests in watch mode
npm run test:watch

# Test coverage
npm run test:cov

# E2E tests
npm run test:e2e
```

### Test Coverage

The project includes comprehensive test coverage for:

- Controllers
- Services
- E2E scenarios

## 🔧 Development

### Available Scripts

```bash
# Development
npm run start:dev          # Start in development mode
npm run start:debug        # Start in debug mode

# Production
npm run build             # Build the application
npm run start:prod        # Start in production mode

# Code Quality
npm run lint              # Run ESLint
npm run format            # Format code with Prettier

# Testing
npm run test              # Run unit tests
npm run test:e2e          # Run E2E tests
```

#### ❗️Important: Running scripts outside of the container can fail if external database is not provided

### Code Style

The project uses:

- **ESLint** for code linting
- **Prettier** for code formatting
- **TypeScript** for type safety

### Database Migrations

The database schema is automatically synchronized using TypeORM's `synchronize: true` option. For production, consider using migrations instead.

## 🐳 Docker

### Docker Compose Services

- **api**: NestJS application
- **db**: PostgreSQL database

### Docker Commands

```bash
# Build and start all services
docker-compose up --build

# Start services in background
docker-compose up -d

# Stop all services
docker-compose down

# Stop all services and remove volume (clear DB)
docker-compose down -v
```

### Database

The container uses postgres, but is compatible with other standard mysql based databases

## 💡 Caveats

### Authentication

The app uses an external endpoint to verify if the user has CUD priviliges for the event entity. While testing the endpoint I realized that it is quite unreliable and changes often (sometimes just commits seppuku and sometimes changes parameters from camel case to snake case). Therefore everything that the API returns other than {"ads": "sure, why not"} is treated as a failed authorization and throws a ForbiddenException.

### Getting the IP address

As the requests coming to the backend are routed through the local network, the public ip is never exposed. Thats why a small helper was built into the frontend that gets the ip from a free service (https://api.ipify.org). The ip is then sent over the network as a header named client-ip. I understand that this is an unsafe practice, as it allows any request to simply fake the header, but an ip can just as easily be changed by routing the request through a VPN, so this approach makes things a bit easier for the development environment.

Nevertheless, this functionality should be removed before deploying to a production scale application. The code for ip handling in those cases is already present.

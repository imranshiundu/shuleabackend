# Shulea Backend API

<div align="center">

![Shulea Backend](https://via.placeholder.com/1200x400/10B981/FFFFFF?text=Shulea+Backend+API)
**RESTful API Server for Modern School Management System**

[![Django](https://img.shields.io/badge/Django-4.2-092E20?style=for-the-badge&logo=django)](https://www.djangoproject.com/)
[![Django REST](https://img.shields.io/badge/DRF-3.14-ff1709?style=for-the-badge&logo=django)](https://www.django-rest-framework.org/)
[![Python](https://img.shields.io/badge/Python-3.11-3776AB?style=for-the-badge&logo=python)](https://www.python.org/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-15-4169E1?style=for-the-badge&logo=postgresql)](https://www.postgresql.org/)
[![Docker](https://img.shields.io/badge/Docker-Ready-2496ED?style=for-the-badge&logo=docker)](https://www.docker.com/)

[![GitHub Issues](https://img.shields.io/github/issues/imranshiundu/shuleabackend?style=flat-square)](https://github.com/imranshiundu/shuleabackend/issues)
[![GitHub Stars](https://img.shields.io/github/stars/imranshiundu/shuleabackend?style=flat-square)](https://github.com/imranshiundu/shuleabackend/stargazers)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=flat-square)](https://opensource.org/licenses/MIT)

[Quick Start](#-quick-start) • [API Documentation](#-api-documentation) • [Development](#-development) • [Deployment](#-deployment)

</div>

## 🌟 Overview

Shulea Backend is a robust, scalable REST API built with Django and Django REST Framework that powers the Shulea School Management System. It provides comprehensive endpoints for managing all aspects of educational institution operations, including student information, academic records, financial management, and communication systems.

### 🎯 Key Features

- **RESTful API Design** - Clean, consistent API endpoints following REST principles
- **Multi-tenant Architecture** - Support for multiple institutions with data isolation
- **Comprehensive Authentication** - JWT-based authentication with role-based access control
- **Real-time Capabilities** - WebSocket support for live updates and notifications
- **Advanced Search & Filtering** - Powerful querying capabilities across all data models
- **File Management** - Secure file upload and storage system
- **Reporting Engine** - Dynamic report generation with multiple export formats

## 🏗️ System Architecture

```mermaid
graph TB
    A[Client Applications] --> B[API Gateway]
    B --> C[Authentication Service]
    B --> D[Core API Services]
    B --> E[File Service]
    
    subgraph "Backend Services"
        C
        D
        E
        F[Background Workers]
        G[Cache Layer]
    end
    
    D --> H[PostgreSQL]
    D --> G
    F --> H
    G --> I[Redis]
    E --> J[Cloud Storage]
    
    subgraph "External Integrations"
        K[Payment Gateways]
        L[SMS Providers]
        M[Email Services]
    end
    
    D --> K
    D --> L
    D --> M
```

## 📦 Project Structure

```
shuleabackend/
├── config/                 # Django project configuration
│   ├── settings/
│   │   ├── base.py        # Base settings
│   │   ├── development.py # Development environment
│   │   └── production.py  # Production environment
│   ├── urls.py            # Main URL routing
│   └── wsgi.py           # WSGI configuration
├── apps/                  # Django applications
│   ├── accounts/         # User authentication & management
│   ├── academics/        # Student, classes, attendance
│   ├── finance/          # Fees, payments, expenses
│   ├── communication/    # Notifications, messaging
│   ├── library/          # Book management
│   ├── inventory/        # Asset tracking
│   └── hr/               # Staff management
├── utils/                # Utility functions & helpers
├── docs/                 # API documentation
├── tests/               # Test suites
├── docker/              # Docker configuration
├── requirements/        # Python dependencies
└── manage.py           # Django management script
```

## 🛠️ Technology Stack

### Core Framework
- **Python 3.11** - Primary programming language
- **Django 4.2** - Web framework with batteries included
- **Django REST Framework 3.14** - Powerful API toolkit
- **Django Channels** - WebSocket support for real-time features

### Database & Storage
- **PostgreSQL 15** - Primary relational database
- **Redis** - Caching and message broker
- **Celery** - Asynchronous task queue
- **Amazon S3** / **DigitalOcean Spaces** - File storage

### Authentication & Security
- **JWT Authentication** - Stateless token-based auth
- **django-cors-headers** - CORS middleware
- **django-axes** - Security and brute force protection
- **python-decouple** - Environment configuration

### API Documentation
- **drf-yasg** - Swagger/OpenAPI documentation
- **django-filter** - Advanced filtering capabilities
- **django-rest-framework-simplejwt** - JWT support

## ⚡ Quick Start

### Prerequisites

- Python 3.11 or higher
- PostgreSQL 12+
- Redis 6+
- Virtualenv (recommended)

### Local Development Setup

1. **Clone the repository**
```bash
git clone https://github.com/imranshiundu/shuleabackend.git
cd shuleabackend
```

2. **Create and activate virtual environment**
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

3. **Install dependencies**
```bash
pip install -r requirements/development.txt
```

4. **Environment configuration**
```bash
cp .env.example .env
# Edit .env with your configuration
```

5. **Database setup**
```bash
# Run migrations
python manage.py migrate

# Create superuser
python manage.py createsuperuser

# Load initial data (optional)
python manage.py loaddata initial_data
```

6. **Start development server**
```bash
python manage.py runserver
```

The API will be available at `http://localhost:8000`

### 🐳 Docker Development

```bash
# Build and start containers
docker-compose -f docker-compose.dev.yml up --build

# Run migrations
docker-compose -f docker-compose.dev.yml exec api python manage.py migrate

# Create superuser
docker-compose -f docker-compose.dev.yml exec api python manage.py createsuperuser
```

## 📚 API Documentation

### Interactive Documentation

Once the server is running, access the API documentation at:

- **Swagger UI**: http://localhost:8000/api/docs/
- **ReDoc**: http://localhost:8000/api/redoc/

### API Endpoints Overview

| Module | Base Endpoint | Description |
|--------|---------------|-------------|
| **Authentication** | `/api/auth/` | JWT token management |
| **Users** | `/api/users/` | User profiles and management |
| **Students** | `/api/students/` | Student information system |
| **Staff** | `/api/staff/` | Teacher and staff management |
| **Academic** | `/api/academics/` | Classes, subjects, timetable |
| **Attendance** | `/api/attendance/` | Student and staff attendance |
| **Finance** | `/api/finance/` | Fees, payments, expenses |
| **Library** | `/api/library/` | Books, loans, inventory |
| **Communication** | `/api/communication/` | Notifications, announcements |

### Authentication Flow

```python
# Obtain JWT tokens
POST /api/auth/login/
{
    "email": "user@school.edu",
    "password": "password"
}

# Response
{
    "access": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9...",
    "refresh": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9...",
    "user": {
        "id": 1,
        "email": "user@school.edu",
        "role": "administrator"
    }
}

# Use token in requests
Authorization: Bearer <access_token>
```

## 🗄️ Data Models

### Core Models Overview

```python
# Simplified model structure
class Institution(models.Model):
    name = models.CharField(max_length=255)
    code = models.CharField(max_length=50, unique=True)
    address = models.TextField()

class User(AbstractUser):
    ROLE_CHOICES = (
        ('admin', 'Administrator'),
        ('teacher', 'Teacher'),
        ('student', 'Student'),
        ('parent', 'Parent'),
    )
    role = models.CharField(max_length=20, choices=ROLE_CHOICES)
    institution = models.ForeignKey(Institution, on_delete=models.CASCADE)

class Student(models.Model):
    user = models.OneToOneField(User, on_delete=models.CASCADE)
    admission_number = models.CharField(max_length=50, unique=True)
    parent = models.ForeignKey('Parent', on_delete=models.SET_NULL, null=True)

class AcademicRecord(models.Model):
    student = models.ForeignKey(Student, on_delete=models.CASCADE)
    class_enrolled = models.ForeignKey('AcademicClass', on_delete=models.CASCADE)
    academic_year = models.CharField(max_length=9)
```

## 🔧 Configuration

### Environment Variables

Create a `.env` file with the following variables:

```ini
# Django
DEBUG=True
SECRET_KEY=your-secret-key-here
ALLOWED_HOSTS=localhost,127.0.0.1

# Database
DATABASE_URL=postgresql://user:password@localhost:5432/shulea

# Redis
REDIS_URL=redis://localhost:6379/0

# Email
EMAIL_BACKEND=django.core.mail.backends.smtp.EmailBackend
EMAIL_HOST=smtp.gmail.com
EMAIL_PORT=587
EMAIL_USE_TLS=True

# File Storage
AWS_ACCESS_KEY_ID=your-access-key
AWS_SECRET_ACCESS_KEY=your-secret-key
AWS_STORAGE_BUCKET_NAME=your-bucket

# External Services
SMS_API_KEY=your-sms-api-key
PAYMENT_GATEWAY_KEY=your-payment-key
```

### Settings Management

The project uses a modular settings structure:

- `config/settings/base.py` - Common settings
- `config/settings/development.py` - Development-specific settings
- `config/settings/production.py` - Production-specific settings

## 🧪 Testing

### Running Tests

```bash
# Run all tests
python manage.py test

# Run specific app tests
python manage.py test apps.accounts

# Run with coverage
coverage run manage.py test
coverage report

# Run specific test case
python manage.py test apps.accounts.tests.test_models
```

### Test Structure

```
tests/
├── unit/           # Unit tests
├── integration/    # Integration tests
├── fixtures/       # Test data fixtures
└── factories.py    # Factory classes for test data
```

## 🚀 Deployment

### Production with Docker

```bash
# Build and deploy
docker-compose -f docker-compose.prod.yml up --build -d

# Run migrations
docker-compose -f docker-compose.prod.yml exec api python manage.py migrate

# Collect static files
docker-compose -f docker-compose.prod.yml exec api python manage.py collectstatic --noinput
```

### Deployment Options

1. **Docker Compose** - Single server deployment
2. **AWS ECS** - Container orchestration
3. **DigitalOcean App Platform** - Platform-as-a-service
4. **Heroku** - Cloud application platform

### Production Checklist

- [ ] Set `DEBUG=False`
- [ ] Configure proper `ALLOWED_HOSTS`
- [ ] Set strong `SECRET_KEY`
- [ ] Configure production database
- [ ] Set up SSL certificates
- [ ] Configure backup strategy
- [ ] Set up monitoring and logging
- [ ] Configure email service
- [ ] Set up file storage

## 📊 Performance Optimization

### Caching Strategy

```python
# Redis caching configuration
CACHES = {
    'default': {
        'BACKEND': 'django_redis.cache.RedisCache',
        'LOCATION': config('REDIS_URL'),
        'OPTIONS': {
            'CLIENT_CLASS': 'django_redis.client.DefaultClient',
        }
    }
}
```

### Database Optimization

- Query optimization with `select_related()` and `prefetch_related()`
- Database indexing on frequently queried fields
- Connection pooling with `django-db-connections`

### Background Tasks

```python
# Celery task example
@app.task
def send_bulk_notifications(notification_data):
    # Process notifications in background
    pass
```

## 🔒 Security

### Security Features

- JWT token authentication with refresh mechanism
- Role-based access control (RBAC)
- CORS configuration for frontend integration
- SQL injection protection through Django ORM
- XSS protection with template auto-escaping
- CSRF protection for state-changing operations
- Rate limiting on authentication endpoints
- Secure password hashing with Argon2

### Security Headers

```python
# Production security settings
SECURE_BROWSER_XSS_FILTER = True
SECURE_CONTENT_TYPE_NOSNIFF = True
X_FRAME_OPTIONS = 'DENY'
SECURE_SSL_REDIRECT = True
SESSION_COOKIE_SECURE = True
CSRF_COOKIE_SECURE = True
```

## 📈 Monitoring & Logging

### Logging Configuration

```python
LOGGING = {
    'version': 1,
    'disable_existing_loggers': False,
    'handlers': {
        'file': {
            'level': 'INFO',
            'class': 'logging.FileHandler',
            'filename': '/var/log/shulea/api.log',
        },
    },
    'loggers': {
        'django': {
            'handlers': ['file'],
            'level': 'INFO',
            'propagate': True,
        },
    },
}
```

### Health Check Endpoint

```http
GET /api/health/
Response:
{
    "status": "healthy",
    "timestamp": "2024-01-15T10:30:00Z",
    "database": "connected",
    "cache": "connected"
}
```

## 🤝 Contributing

We welcome contributions! Please see our [Contributing Guide](CONTRIBUTING.md) for details.

### Development Workflow

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Code Style

```bash
# Format code with black
black .

# Sort imports with isort
isort .

# Check code quality
flake8
```

## 🐛 Troubleshooting

### Common Issues

1. **Database connection errors**
   - Check PostgreSQL is running
   - Verify DATABASE_URL in .env file

2. **Migration conflicts**
   ```bash
   python manage.py makemigrations
   python manage.py migrate
   ```

3. **Static files not loading**
   ```bash
   python manage.py collectstatic
   ```

4. **Redis connection issues**
   - Ensure Redis server is running
   - Check REDIS_URL configuration

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- Django and Django REST Framework communities
- All contributors and testers
- Educational institutions providing feedback
- Open source libraries that make this project possible

---

<div align="center">

**Shulea Backend API** - *Powering the Future of Education Management*

[Frontend Repository](https://github.com/imranshiundu/shulea) • [Documentation](https://docs.shulea.app) • [Report Bug](https://github.com/imranshiundu/shuleabackend/issues) • [Request Feature](https://github.com/imranshiundu/shuleabackend/issues)

</div>

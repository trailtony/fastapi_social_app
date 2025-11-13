# FastAPI Social Media Application

A full-stack social media platform built with FastAPI (backend) and Streamlit (frontend), featuring user authentication, media uploads to CDN, and a chronological feed.

## 🚀 Features

- **User Authentication**: JWT-based authentication with registration, login, and password reset
- **Media Uploads**: Upload images and videos with captions
- **CDN Integration**: ImageKit for media hosting with on-the-fly transformations
- **Social Feed**: Chronological feed of all posts with user attribution
- **Ownership Controls**: Users can only delete their own posts
- **Auto-Generated API Docs**: OpenAPI/Swagger UI at `/docs`

## 🛠️ Tech Stack

### Backend
- **FastAPI**: Modern, fast web framework with async support
- **SQLAlchemy**: Async ORM for database operations
- **SQLite**: Lightweight database (aiosqlite for async support)
- **fastapi-users**: Complete user authentication system
- **ImageKit**: CDN for image/video storage and transformations
- **Uvicorn**: ASGI server with hot reload

### Frontend
- **Streamlit**: Python-based UI framework for rapid prototyping

## 📋 Prerequisites

- Python 3.12+
- ImageKit account (free tier available at [imagekit.io](https://imagekit.io))

## 🔧 Installation

### 1. Clone the repository
```pwsh
git clone <repository-url>
cd fastapi_social_app
```

### 2. Install dependencies

Using `uv` (recommended):
```pwsh
uv sync
```

Or using `pip`:
```pwsh
pip install -e .
```

### 3. Set up environment variables

Create a `.env` file in the project root:
```env
IMAGEKIT_PRIVATE_KEY=your_private_key_here
IMAGEKIT_PUBLIC_KEY=your_public_key_here
IMAGEKIT_URL=https://ik.imagekit.io/your_id
```

To get ImageKit credentials:
1. Sign up at [imagekit.io](https://imagekit.io)
2. Go to Developer Options in your dashboard
3. Copy your Private Key, Public Key, and URL endpoint

## 🏃 Running the Application

### Start the Backend API
```pwsh
python main.py
```
API will be available at: http://localhost:8000

### Start the Frontend (in a new terminal)
```pwsh
streamlit run frontend.py
```
Frontend will be available at: http://localhost:8501

## 📖 API Documentation

Once the backend is running, access:
- **Swagger UI**: http://localhost:8000/docs
- **ReDoc**: http://localhost:8000/redoc

## 🔑 API Endpoints

### Authentication
- `POST /auth/register` - Register new user
- `POST /auth/jwt/login` - Login and get JWT token
- `POST /auth/forgot-password` - Request password reset
- `POST /auth/reset-password` - Reset password with token

### Users
- `GET /users/me` - Get current user info
- `PATCH /users/me` - Update current user

### Posts
- `POST /upload` - Upload image/video with caption
- `GET /feed` - Get all posts (newest first)
- `DELETE /posts/{post_id}` - Delete own post

## 📁 Project Structure

```
fastapi_social_app/
├── app/
│   ├── app.py          # Main FastAPI application and routes
│   ├── db.py           # Database models and session management
│   ├── users.py        # Authentication setup (fastapi-users)
│   ├── images.py       # ImageKit client configuration
│   └── schemas.py      # Pydantic models for API validation
├── frontend.py         # Streamlit UI application
├── main.py             # Development server launcher
├── pyproject.toml      # Project dependencies and metadata
├── WARP.md             # Development guide for WARP AI
├── TECHNICAL_GUIDE.md  # Comprehensive technical documentation
└── README.md           # This file
```

## 🎯 Usage Flow

1. **Register**: Create an account with email and password
2. **Login**: Authenticate to receive JWT token (stored automatically)
3. **Upload**: Navigate to Upload page, select media file, add caption, and share
4. **Feed**: View all posts from all users in chronological order
5. **Delete**: Delete your own posts using the trash icon

## 🔒 Security Notes

⚠️ **For Development/Demo Only**:
- JWT secret is hardcoded (should be in environment variables)
- No rate limiting implemented
- Password reset tokens print to console (no email integration)
- SQLite doesn't handle concurrent writes well

**For Production**, consider:
- Move JWT secret to environment variables
- Switch to PostgreSQL for better concurrency
- Add CORS configuration
- Implement rate limiting
- Add file validation (size, type, malware scanning)
- Enable HTTPS
- Integrate email service (SendGrid, etc.)

## 🧪 Testing

Currently, no automated tests are implemented. To add tests:

```pwsh
# Install test dependencies
pip install pytest pytest-asyncio httpx

# Run tests
pytest
```

Example test structure:
```python
import pytest
from httpx import AsyncClient
from app.app import app

@pytest.mark.asyncio
async def test_feed_requires_auth():
    async with AsyncClient(app=app, base_url="http://test") as client:
        response = await client.get("/feed")
        assert response.status_code == 401
```

## 📚 Learning Resources

This project demonstrates:
- **Async Python**: `async`/`await` patterns with ASGI
- **Dependency Injection**: FastAPI's `Depends()` system
- **ORM**: SQLAlchemy 2.0 with async support
- **JWT Authentication**: Stateless token-based auth
- **RESTful API Design**: Resource-based endpoints
- **CDN Integration**: Third-party service integration
- **File Upload Handling**: Multipart form data processing

See `TECHNICAL_GUIDE.md` for in-depth explanations of concepts, algorithms, and design patterns.

## 🤝 Contributing

This is a learning/demo project. Suggestions for improvements:
- Add pagination to feed endpoint
- Implement post likes/comments
- Add user profiles
- Add search functionality
- Implement post editing
- Add database migrations (Alembic)
- Create comprehensive test suite

## 📄 License

MIT License - feel free to use this project for learning and portfolio purposes.

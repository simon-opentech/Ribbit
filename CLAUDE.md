# Ribbit - Django + React Reddit Clone

This is a full-stack Reddit clone called Ribbit, built with Django REST Framework (backend) and React + Redux (frontend).

## Project Structure
- `backend/` - Django REST API with MySQL database
- `frontend/` - React.js frontend with Redux state management

## Development Commands

### Backend (Django)
```bash
cd backend
python manage.py runserver        # Start Django dev server
python manage.py test            # Run backend tests
python manage.py makemigrations  # Create database migrations  
python manage.py migrate         # Apply database migrations
```

### Frontend (React)
```bash
cd frontend
npm start                        # Start React dev server
npm run build                    # Build for production
npm test                         # Run frontend tests
```

### Database
- Uses MySQL in production
- SQLite for local development (if configured)
- Database settings in `backend/backend/settings.py`

## Key Features
- User authentication with JWT tokens
- Posts with voting (upvote/downvote)
- Communities (subreddits equivalent)
- Comments with nested replies
- User profiles and notifications
- Real-time updates

## API Architecture
- Django REST Framework with modular structure
- Organized by feature modules (Auth, Post, Comment, Subribbit, etc.)
- Each module has Actions, Views, Serializers, and Validators
- JWT authentication using Simple JWT

## Frontend Architecture
- React with Redux for state management
- Component-based architecture
- Bootstrap for styling
- Axios for API calls
- React Router for navigation

## Testing
- Backend: Django unit tests in `backend/base/tests/`
- Frontend: React Testing Library
- CI/CD with GitHub Actions

## Docker Support
- Dockerized full-stack application available
- Can be run with single Docker command

## Database Models
Key models include:
- User, UserProfile
- Post, PostVote  
- Comment, CommentLike
- Subribbit, SubribbitMember
- Notification

## Security
- JWT token authentication
- CORS headers configured
- Input validation and sanitization
- Password reset functionality

## Deployment
- Deployed on Digital Ocean
- Uses MySQL database
- Dockerized deployment
# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Architecture Overview

Ribbit is a full-stack Reddit clone with a Django REST API backend and React + Redux frontend.

**Backend Structure:**
- Django REST Framework with Simple JWT authentication
- Modular architecture under `backend/base/modules/`
- Each module follows a consistent pattern: `Api/`, `Data/`, `Actions/`, `Serializers/`, `Validators/`
- Core modules: Auth, Comment, Post, PostVote, Subribbit, User, Notification
- Models are centrally imported in `backend/base/models.py`

**Frontend Structure:**
- React 17 with Redux for state management
- Component-based architecture organized by feature
- Key directories: `components/`, `actions/`, `reducers/`, `securityUtils/`
- Bootstrap 5 for styling

## Development Commands

**Backend (Django):**
```bash
cd backend
python manage.py runserver          # Start development server
python manage.py test               # Run all tests
python manage.py makemigrations     # Create migrations
python manage.py migrate            # Apply migrations
```

**Frontend (React):**
```bash
cd frontend
npm start                          # Start development server
npm run build                      # Build for production
npm test                          # Run tests
```

**Database Setup:**
- Copy `backend/settingsExample.py` to `backend/settings.py`
- Configure MySQL credentials in the DATABASE section
- Uses MySQL in production, SQLite for testing

## Key Patterns

**Django Module Structure:**
- `Actions/` - Business logic classes
- `Api/` - ViewSets and API endpoints
- `Data/` - Model definitions
- `Serializers/` - DRF serializers
- `Validators/` - Request validation classes

**Authentication:**
- Uses Django REST Framework Simple JWT
- Token-based authentication with refresh tokens
- Password reset functionality via email

**Testing:**
- Backend tests located in `backend/base/tests/`
- GitHub Actions CI runs tests on push/PR to main
- Tests run with: `python manage.py test`

## Docker Support

The project includes Docker support with a full-stack container available at:
`ghcr.io/juliantjg/ribbit-full-stack:latest`

## Environment Configuration

Settings file must be created from template:
```bash
mv backend/settingsExample.py backend/settings.py
```

Configure database, email, and other environment-specific settings in `settings.py`.
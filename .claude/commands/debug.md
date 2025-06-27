# Debug Command

Common debugging commands and troubleshooting.

## Django Debug
```bash
cd backend

# Check for issues
python manage.py check

# View migrations
python manage.py showmigrations

# Django shell for debugging
python manage.py shell

# View database schema
python manage.py inspectdb

# Reset database (development only)
find . -path "*/migrations/*.py" -not -name "__init__.py" -delete
find . -path "*/migrations/*.pyc" -delete
python manage.py makemigrations
python manage.py migrate
```

## React Debug
```bash
cd frontend

# Check for dependency issues
npm audit

# Clear cache
npm cache clean --force

# Reinstall dependencies
rm -rf node_modules package-lock.json
npm install

# Start with verbose logging
npm start --verbose
```

## Common Issues
- **CORS errors**: Check `django-cors-headers` settings in Django
- **JWT token issues**: Check token expiration and refresh logic
- **Database connection**: Verify MySQL credentials in `settings.py`
- **API 404 errors**: Check URL patterns in Django `urls.py` files
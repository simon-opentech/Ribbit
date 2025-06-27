# Test All Command

Runs all tests for both backend and frontend.

## Usage
```bash
# Run backend tests
cd backend && python manage.py test

# Run frontend tests  
cd frontend && npm test

# Run tests with coverage (if configured)
cd backend && python manage.py test --with-coverage
cd frontend && npm test -- --coverage
```

## Test Files
- Backend tests: `backend/base/tests/`
- Frontend tests: `frontend/src/` (files ending in `.test.js`)
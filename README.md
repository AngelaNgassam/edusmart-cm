# EDUSMART-CM

Projet académique — Module [Administration - Enseignant]

## Structure
- `backend/` — API Laravel
- `frontend/` — React Vite

## Installation

### Backend
bash
cd backend
cp .env.example .env
composer install
php artisan key:generate
php artisan migrate
php artisan serve


### Frontend
bash
cd frontend
npm install
npm run dev


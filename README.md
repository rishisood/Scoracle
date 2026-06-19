# Scoracle - Premier League Prediction Game

A web application that allows users to predict Premier League match scores, track their predictions, earn points, and compete on a global leaderboard.

## 🎯 Features

### User Experience
- **Fixture Display**: View all Premier League fixtures with club crests and match details
- **Predictions**: Make score predictions for each match up to 24 hours before the first match of the week
- **Prediction Lock**: Automatic lock on Thursday before midweek matches; manual lock option available
- **Email Notifications**: 
  - Confirmation email when predictions are locked
  - Weekly digest email with all match predictions
- **Filtering**: Filter fixtures by club and month
- **Responsive Design**: Fully responsive for both desktop and mobile web browsers

### Scoring & Leaderboard
- **Point System**: 1 point for correct predictions, 0 points for incorrect
- **Live Dashboard**: Real-time leaderboard showing user rankings by points
- **Match Details**: Collapsible cards showing each player's predictions and accuracy

### Admin Features
- **User Management**: Admin panel for managing user accounts
- **Force Password Change**: Required password change on first admin login
- **User Verification**: Manage user accounts and send verification emails

## 🛠️ Tech Stack

### Frontend
- **HTML5 / CSS3 / JavaScript (Vanilla)**
- Responsive design for desktop and mobile
- Live updates using WebSocket or polling

### Backend
- **Runtime**: Node.js or Python (to be determined)
- **API**: RESTful API for frontend consumption
- **Authentication**: JWT-based authentication with encrypted passwords

### Database
- **SQLite**: Local database for storing users, predictions, and scores

### Infrastructure
- **Hosting**: Netlify (Free Tier)
- **Database & Auth**: Supabase (Free Tier) - PostgreSQL alternative to SQLite
- **Email Service**: Resend (Free Tier) - Transactional emails
- **External API**: Premier League official API (for fixtures, results, standings, club data)

## 📊 Database Schema (Overview)

```
Users
├── id (PK)
├── email (unique)
├── username (unique)
├── password_hash
├── created_at
└── is_admin

Fixtures
├── id (PK)
├── pl_fixture_id (external API ID)
├── home_club_id
├── away_club_id
├── scheduled_date
├── status
├── home_score
├── away_score
└── week_number

Predictions
├── id (PK)
├── user_id (FK)
├── fixture_id (FK)
├── predicted_home_score
├── predicted_away_score
├── is_correct (null/true/false)
├── points_earned
└── created_at

Clubs
├── id (PK)
├── pl_club_id
├── name
├── crest_url
└── short_name
```

## 🚀 Getting Started

### Prerequisites
- Node.js (v16+) or Python (3.8+)
- Git
- A Supabase account
- A Resend account
- Netlify account for deployment

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/rishisood/Scoracle.git
   cd Scoracle
   ```

2. **Setup Environment Variables**
   Create a `.env` file in the root directory:
   ```
   # Database
   SUPABASE_URL=your_supabase_url
   SUPABASE_KEY=your_supabase_key
   
   # Email
   RESEND_API_KEY=your_resend_key
   
   # App Config
   JWT_SECRET=your_jwt_secret
   NODE_ENV=development
   PORT=3000
   
   # PL API
   PL_API_BASE_URL=https://www.api.premierleague.com
   ```

3. **Install Dependencies** (Backend)
   ```bash
   npm install
   # or
   pip install -r requirements.txt
   ```

4. **Initialize Database**
   ```bash
   npm run migrate
   # or
   python manage.py migrate
   ```

5. **Start Development Server**
   ```bash
   npm run dev
   # or
   python runserver.py
   ```

6. **Access the Application**
   - Frontend: `http://localhost:3000`
   - Admin: `/admin`

## 📋 API Endpoints

### Authentication
- `POST /api/auth/register` - Register new user
- `POST /api/auth/login` - Login user
- `POST /api/auth/change-password` - Change password (admin first login)

### Fixtures & Predictions
- `GET /api/fixtures` - Get all fixtures (with filters)
- `GET /api/fixtures/:id` - Get specific fixture
- `POST /api/predictions` - Create prediction
- `PUT /api/predictions/:id` - Update prediction
- `GET /api/predictions/user/:userId` - Get user predictions

### Leaderboard
- `GET /api/leaderboard` - Get all users ranked by points
- `GET /api/leaderboard/week/:week` - Get weekly leaderboard

### Admin
- `GET /api/admin/users` - List all users
- `DELETE /api/admin/users/:id` - Delete user
- `POST /api/admin/users/:id/verify` - Verify user email

## 🔐 Security

- **Password Encryption**: Bcrypt for password hashing
- **JWT Authentication**: Secure token-based authentication
- **Email Verification**: Two-step verification for new accounts
- **Admin Only Routes**: Protected endpoints for admin operations
- **HTTPS**: Enforced on production (Netlify)
- **CORS**: Properly configured for frontend domain

## 📧 Email Templates

1. **Registration Confirmation**: Welcome email with verification link
2. **Prediction Lock**: Reminder with locked predictions summary
3. **Weekly Digest**: Summary of all week's predictions
4. **Leaderboard Update**: Optional weekly standings update
5. **Admin Notifications**: Account changes and security events

## 🌐 External API Integration

### Premier League API
- Get fixtures for upcoming weeks
- Fetch club data (names, crests, badges)
- Retrieve match results and final scores
- Get league standings and statistics

**Endpoints Used**:
- `/fixtures?status=SCHEDULED` - Upcoming matches
- `/fixtures/:id` - Match details
- `/teams` - Club information
- `/standings` - League table

## 📱 Responsive Design Breakpoints

- **Mobile**: 320px - 767px
- **Tablet**: 768px - 1023px
- **Desktop**: 1024px+

## 🔄 Weekly Workflow

**Monday - Wednesday**
- Users view upcoming fixtures
- Users make/update predictions
- Predictions remain unlocked

**Thursday (Morning)**
- Automatic prediction lock at specified time
- Confirmation emails sent
- Digest emails sent
- Leaderboard updated with locked status

**Friday - Sunday**
- Predictions locked (read-only)
- Users can view predictions only
- Matches are played

**Monday (After Matches)**
- Results synced from PL API
- Prediction accuracy calculated
- Points awarded
- Leaderboard updated

## 🤝 Contributing

1. Create a feature branch from `dev`
   ```bash
   git checkout -b feature/your-feature-name
   ```

2. Commit your changes with clear messages
   ```bash
   git commit -m "feat: add prediction locking mechanism"
   ```

3. Push to your branch
   ```bash
   git push origin feature/your-feature-name
   ```

4. Create a Pull Request to the `dev` branch

## 📝 Branch Strategy

- **main**: Production-ready code
- **dev**: Development branch for integration testing
- **feature/\***: Feature branches (merge to dev)
- **hotfix/\***: Urgent fixes (merge to main and dev)

## 🐛 Reporting Issues

Please create an issue on GitHub with:
- Description of the bug
- Steps to reproduce
- Expected vs actual behavior
- Screenshots/logs if applicable

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 👨‍💼 Admin Setup

### Initial Admin Creation
The first admin user should be created manually in the database or via a setup script with the following credentials:
```
Username: admin (changeable)
Password: [temporary_password_provided_separately]
```

**First Login Requirements**:
1. User must change the default password
2. User can then access `/admin` dashboard
3. Admin can manage user accounts and view system logs

## 🎨 UI/UX Features

- **Dark Mode Support**: Light and dark theme options
- **Accessibility**: WCAG 2.1 AA compliance
- **Loading States**: Smooth skeleton loaders and spinners
- **Error Handling**: User-friendly error messages
- **Toast Notifications**: Feedback for user actions
- **Real-time Updates**: Live leaderboard updates

## 📞 Support

For questions or support, please open an issue on GitHub or contact the development team.

---

**Status**: In Development 🚧

Last Updated: June 2026

# HỆ THỐNG QUẢN LÝ VÀ XEM PHIM TRỰC TUYẾN

Hệ thống quản lý và xem phim trực tuyến được xây dựng theo kiến trúc hướng dịch vụ (SOA - Service Oriented Architecture), cung cấp nền tảng giải trí đa phương tiện với các tính năng quản lý nội dung, thanh toán, và tương tác người dùng.

## MỤC LỤC

- [Tổng quan](#tổng-quan)
- [Tính năng chính](#tính-năng-chính)
- [Công nghệ sử dụng](#công-nghệ-sử-dụng)
- [Yêu cầu hệ thống](#yêu-cầu-hệ-thống)
- [Cài đặt và chạy dự án](#cài-đặt-và-chạy-dự-án)
- [Cấu trúc dự án](#cấu-trúc-dự-án)
- [Kiến trúc hệ thống](#kiến-trúc-hệ-thống)
- [Database Schema](#database-schema)
- [API Endpoints](#api-endpoints)
- [Cấu hình môi trường](#cấu-hình-môi-trường)
- [Tài khoản demo](#tài-khoản-demo)
- [Troubleshooting](#troubleshooting)
- [Thành viên nhóm](#thành-viên-nhóm)

## TỔNG QUAN

Dự án xây dựng hệ thống quản lý và xem phim trực tuyến toàn diện, ứng dụng kiến trúc microservices để đảm bảo khả năng mở rộng và bảo trì. Hệ thống cung cấp:

- Xem phim trực tuyến với phân loại theo độ tuổi
- Quản lý người dùng với hệ thống Premium
- Ví điện tử và thanh toán tự động
- Đánh giá, bình luận và bộ sưu tập cá nhân
- Tìm kiếm nâng cao và gợi ý thông minh
- Quản trị viên với dashboard analytics
- Tích hợp thanh toán ngân hàng (Casso API)

## TÍNH NĂNG CHÍNH

### 1. Xác thực và Bảo mật

- Đăng ký/Đăng nhập với xác thực email
- Xác thực OTP qua email với rate limiting
- JWT Authentication với token refresh
- Password hashing với passlib
- Rate limiting để chống spam và DDoS
- Token blacklist khi logout
- Protected routes và middleware authorization
- Kiểm tra độ tuổi cho nội dung hạn chế

### 2. Quản lý Người dùng

- Profile cá nhân với avatar và thông tin chi tiết
- Ví điện tử tích hợp
- Nạp tiền tự động qua chuyển khoản ngân hàng
- Hệ thống Premium với nhiều gói
- Lịch sử giao dịch và thanh toán
- Quản lý ngày sinh và xác thực độ tuổi
- Danh sách yêu thích và bộ sưu tập

### 3. Quản lý Phim và Nội dung

#### Người dùng:

- Xem phim trực tuyến HD
- Tìm kiếm nâng cao (theo tên, thể loại, độ tuổi, năm phát hành)
- Lọc phim (anime, featured, trending, age rating)
- Đánh giá phim (1-5 sao)
- Bình luận và thảo luận
- Tạo bộ sưu tập cá nhân
- Lịch sử xem phim

#### Admin:

- CRUD phim đầy đủ
- Quản lý featured movies và hero banner
- Quản lý độ tuổi phù hợp
- Kiểm duyệt bình luận
- Dashboard analytics realtime
- Quản lý người dùng (ban, unban, adjust wallet)
- Xem báo cáo và thống kê

### 4. Hệ thống Thanh toán

- Ví điện tử với số dư khả dụng
- Nạp tiền tự động qua Casso API
- Webhook xác thực giao dịch
- QR Code thanh toán
- Lịch sử giao dịch chi tiết
- Mua Premium tự động
- Xử lý đồng thời với transaction lock

### 5. Bộ sưu tập và Tương tác

- Tạo bộ sưu tập phim cá nhân
- Public/Private collections
- Like phim yêu thích
- Comment và rating
- Tổng hợp thống kê (views, likes, ratings)

### 6. Giao diện Người dùng

- Responsive design hoàn chỉnh
- Dark mode theme
- Search với autocomplete
- Infinite scroll cho danh sách phim
- Modal và notification thông minh
- Loading states và error handling
- Smooth animations và transitions

## CÔNG NGHỆ SỬ DỤNG

### Frontend Framework & Libraries

| Công nghệ        | Phiên bản | Mô tả                       |
| ---------------- | --------- | --------------------------- |
| React            | 19.1.1    | UI Framework                |
| TypeScript       | 5.9.3     | Type safety & Development   |
| Vite             | 7.1.7     | Build tool & Dev server     |
| React Router DOM | 7.9.5     | Client-side routing         |
| TailwindCSS      | 4.1.16    | Utility-first CSS framework |
| Zustand          | 5.0.8     | State management            |
| React Query      | 5.90.7    | Data fetching & caching     |
| React Hook Form  | 7.66.0    | Form validation             |
| Axios            | 1.13.2    | HTTP client                 |
| Zod              | 4.1.12    | Schema validation           |
| Lucide React     | 0.553.0   | Icon library                |
| React Hot Toast  | 2.6.0     | Toast notifications         |
| QRCode           | 1.5.4     | QR code generation          |
| date-fns         | 4.1.0     | Date manipulation           |

### Backend Framework & Libraries

| Công nghệ        | Phiên bản | Mục đích                  |
| ---------------- | --------- | ------------------------- |
| Python           | 3.8+      | Backend runtime           |
| FastAPI          | 0.120.0   | REST API framework        |
| Uvicorn          | 0.38.0    | ASGI server               |
| Pydantic         | 2.11.0    | Data validation           |
| Motor            | 3.7.1     | Async MongoDB driver      |
| PyMongo          | Latest    | MongoDB driver            |
| python-jose      | 3.3.0     | JWT token handling        |
| Passlib          | 1.7.4     | Password hashing          |
| SlowAPI          | 0.1.9     | Rate limiting             |
| Redis            | 5.0.8     | Caching & session storage |
| aiosmtplib       | 3.0.1     | Async email sending       |
| python-multipart | 0.0.6     | Form data handling        |
| python-dotenv    | 1.0.1     | Environment variables     |

### Database & Storage

| Công nghệ | Phiên bản | Mục đích                 |
| --------- | --------- | ------------------------ |
| MongoDB   | 5.0+      | NoSQL Database (primary) |
| Redis     | 7.0+      | Caching & rate limiting  |

### Development Tools

| Công nghệ         | Phiên bản | Mục đích                      |
| ----------------- | --------- | ----------------------------- |
| ESLint            | 9.36.0    | Code linting                  |
| TypeScript ESLint | 8.45.0    | TypeScript linting            |
| Vite Plugin React | 5.0.4     | React Fast Refresh            |
| Docker            | Latest    | Containerization              |
| Docker Compose    | Latest    | Multi-container orchestration |

### Deployment & DevOps

| Platform      | Mục đích            |
| ------------- | ------------------- |
| Railway       | Backend hosting     |
| Vercel        | Frontend hosting    |
| MongoDB Atlas | Production database |

### Key Features Implementation

- **State Management**: Zustand stores với persist middleware
- **Data Fetching**: React Query với caching strategies
- **Authentication**: JWT with refresh tokens
- **Real-time**: Webhook integration với Casso
- **File Upload**: Multipart form data
- **Email Service**: SMTP với async sending
- **Rate Limiting**: Per-endpoint với SlowAPI
- **Validation**: Zod schemas và Pydantic models
- **Security**: CORS, rate limiting, token blacklist
- **Monitoring**: Health check endpoints

## YÊU CẦU HỆ THỐNG

### Frontend

```bash
Node.js >= 16.0.0
npm >= 8.0.0
```

### Backend

```bash
Python >= 3.8
pip >= 21.0
MongoDB >= 5.0
Redis >= 7.0 (optional, cho caching)
```

### Database

```bash
MongoDB >= 5.0 (local hoặc Atlas)
MongoDB Compass (optional, GUI tool)
```

## CÀI ĐẶT VÀ CHẠY DỰ ÁN

### 1. Clone dự án

```bash
git clone https://github.com/JasonNguyen31/finalsoa.git
cd finalsoa
```

### 2. Cài đặt MongoDB

#### Option A: MongoDB Local

**Windows:**

1. Tải MongoDB Community Server từ [mongodb.com](https://www.mongodb.com/try/download/community)
2. Cài đặt và chạy MongoDB Service
3. Mặc định chạy tại `mongodb://localhost:27017`

**macOS (Homebrew):**

```bash
brew tap mongodb/brew
brew install mongodb-community@7.0
brew services start mongodb-community@7.0
```

**Linux (Ubuntu):**

```bash
sudo apt-get install gnupg curl
curl -fsSL https://www.mongodb.org/static/pgp/server-7.0.asc | \
   sudo gpg -o /usr/share/keyrings/mongodb-server-7.0.gpg \
   --dearmor
echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list
sudo apt-get update
sudo apt-get install -y mongodb-org
sudo systemctl start mongod
```

#### Option B: MongoDB Atlas (Cloud)

1. Đăng ký tài khoản miễn phí tại [mongodb.com/cloud/atlas](https://www.mongodb.com/cloud/atlas)
2. Tạo cluster mới (Free M0)
3. Tạo database user
4. Lấy connection string
5. Cập nhật `MONGODB_URL` trong file `.env` của các services

### 3. Cài đặt Redis (Optional - cho caching)

**macOS:**

```bash
brew install redis
brew services start redis
```

**Ubuntu:**

```bash
sudo apt-get update
sudo apt-get install redis-server
sudo systemctl start redis-server
```

**Windows:**

- Tải từ [github.com/microsoftarchive/redis/releases](https://github.com/microsoftarchive/redis/releases)
- Hoặc dùng WSL2

### 4. Cài đặt Dependencies

#### Frontend:

```bash
npm install
```

#### Backend - Từng service:

**Cách 1: Cài đặt cho tất cả services**

```bash
cd backend

# Auth Service
cd auth_service
pip install -r requirements.txt
cd ..

# User Service
cd user_service
pip install -r requirements.txt
cd ..

# Movie Service
cd movie_service
pip install -r requirements.txt
cd ..

# Collection Service
cd collection_service
pip install -r requirements.txt
cd ..

# Book Service
cd book_service
pip install -r requirements.txt
cd ..
```

**Cách 2: Sử dụng virtual environment (Khuyến nghị)**

```bash
# Tạo venv cho từng service
cd backend/auth_service
python -m venv venv
source venv/bin/activate  # macOS/Linux
# hoặc
venv\Scripts\activate     # Windows
pip install -r requirements.txt
deactivate

# Lặp lại cho các services khác
```

### 5. Cấu hình Environment Variables

#### Frontend (.env):

Tạo file `.env` trong root folder:

```bash
# Backend Microservices
VITE_BACKEND_HOST=http://localhost
VITE_AUTH_SERVICE_URL=http://localhost:8001
VITE_USER_SERVICE_URL=http://localhost:8002/api/user
VITE_MOVIE_SERVICE_URL=http://localhost:8003
VITE_BOOK_SERVICE_URL=http://localhost:8004
VITE_COLLECTION_SERVICE_URL=http://localhost:8005

# App Configuration
VITE_APP_NAME=MovieHub
VITE_ENV=development
```

#### Backend (.env cho mỗi service):

**auth_service/.env:**

```bash
MONGODB_URL=mongodb://localhost:27017
DB_NAME=finalsoa_auth
JWT_SECRET_KEY=your-super-secret-jwt-key-change-in-production
JWT_ALGORITHM=HS256
ACCESS_TOKEN_EXPIRE_MINUTES=30
REFRESH_TOKEN_EXPIRE_DAYS=7

# Email Configuration
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USER=your-email@gmail.com
SMTP_PASSWORD=your-app-password
SMTP_FROM=your-email@gmail.com
```

**user_service/.env:**

```bash
MONGODB_URL=mongodb://localhost:27017
DB_NAME=finalsoa_user
JWT_SECRET_KEY=your-super-secret-jwt-key-change-in-production
JWT_ALGORITHM=HS256

# Casso API (cho thanh toán tự động)
CASSO_API_KEY=your-casso-api-key
CASSO_WEBHOOK_SECRET=your-webhook-secret
```

**movie_service/.env:**

```bash
MONGODB_URL=mongodb://localhost:27017
DB_NAME=finalsoa_movie
JWT_SECRET_KEY=your-super-secret-jwt-key-change-in-production
JWT_ALGORITHM=HS256
```

**collection_service/.env:**

```bash
MONGODB_URL=mongodb://localhost:27017
DB_NAME=finalsoa_collection
JWT_SECRET_KEY=your-super-secret-jwt-key-change-in-production
JWT_ALGORITHM=HS256
```

**book_service/.env:**

```bash
MONGODB_URL=mongodb://localhost:27017
DB_NAME=finalsoa_book
JWT_SECRET_KEY=your-super-secret-jwt-key-change-in-production
JWT_ALGORITHM=HS256
```

> **⚠️ LƯU Ý BẢO MẬT:**
>
> - JWT_SECRET_KEY phải giống nhau ở tất cả services
> - Email SMTP_PASSWORD phải dùng App Password (Gmail)
> - KHÔNG commit file .env lên Git
> - Thay đổi tất cả secrets trong production

### 6. Tạo Admin Account

```bash
cd backend
python create_admin.py
```

Script sẽ hỏi thông tin:

```
Enter admin username: admin
Enter admin email: admin@example.com
Enter admin password: ********
Confirm password: ********
```

### 7. Chạy Development Servers

#### Option A: Chạy từng service thủ công

**Mở 6 terminal tabs:**

**Tab 1 - Frontend:**

```bash
npm run dev
```

✅ Frontend: `http://localhost:5173`

**Tab 2 - Auth Service:**

```bash
cd backend/auth_service
python -m uvicorn app.main:app --reload --port 8001
```

✅ Auth Service: `http://localhost:8001`

**Tab 3 - User Service:**

```bash
cd backend/user_service
python -m uvicorn app.main:app --reload --port 8002
```

✅ User Service: `http://localhost:8002`

**Tab 4 - Movie Service:**

```bash
cd backend/movie_service
python -m uvicorn app.main:app --reload --port 8003
```

✅ Movie Service: `http://localhost:8003`

**Tab 5 - Collection Service:**

```bash
cd backend/collection_service
python -m uvicorn app.main:app --reload --port 8005
```

✅ Collection Service: `http://localhost:8005`

**Tab 6 - Book Service:**

```bash
cd backend/book_service
python -m uvicorn app.main:app --reload --port 8004
```

✅ Book Service: `http://localhost:8004`

#### Option B: Sử dụng script tự động (macOS/Linux)

```bash
cd backend
chmod +x start_all_services.sh
./start_all_services.sh
```

Script sẽ tự động:

- Kill các process cũ trên ports 8001-8005
- Start tất cả 5 backend services
- Log output vào `/tmp/[service_name].log`

**Kiểm tra status:**

```bash
lsof -i:8001,8002,8003,8004,8005
```

**Xem logs:**

```bash
tail -f /tmp/auth_service.log
tail -f /tmp/user_service.log
tail -f /tmp/movie_service.log
```

**Stop tất cả services:**

```bash
kill -9 $(lsof -ti:8001,8002,8003,8004,8005)
```

#### Option C: Sử dụng Docker Compose

```bash
cd backend
docker-compose up -d
```

Services sẽ chạy tại các ports như trên.

**Stop containers:**

```bash
docker-compose down
```

### 8. Kiểm tra Backend APIs

- **Auth Service API Docs**: http://localhost:8001/docs
- **User Service API Docs**: http://localhost:8002/docs
- **Movie Service API Docs**: http://localhost:8003/docs
- **Collection Service API Docs**: http://localhost:8005/docs
- **Book Service API Docs**: http://localhost:8004/docs

### 9. Build Production

#### Frontend:

```bash
npm run build
```

Production files sẽ trong folder `dist/`

#### Backend:

```bash
# Build Docker images
cd backend
docker-compose -f docker-compose.prod.yml build
docker-compose -f docker-compose.prod.yml up -d
```

### Scripts khác

```bash
# Frontend
npm run lint          # Lint code
npm run preview       # Preview production build

# Backend (trong mỗi service)
python -m pytest      # Run tests (nếu có)
```

## CẤU TRÚC DỰ ÁN

```
finalsoa/
├── backend/                      # Backend microservices
│   ├── auth_service/            # Authentication service (Port 8001)
│   │   ├── app/
│   │   │   ├── main.py         # FastAPI entry point
│   │   │   ├── core/           # Core configurations
│   │   │   │   ├── config.py   # Environment config
│   │   │   │   ├── database.py # MongoDB connection
│   │   │   │   ├── limiter.py  # Rate limiting
│   │   │   │   ├── otp_limiter.py # OTP rate limiting
│   │   │   │   ├── response.py # Response formatter
│   │   │   │   └── token_blacklist.py # JWT blacklist
│   │   │   ├── middlewares/
│   │   │   │   └── jwt_middleware.py # JWT verification
│   │   │   ├── routes/
│   │   │   │   └── auth_routes.py # Auth endpoints
│   │   │   ├── controllers/
│   │   │   │   └── auth_controller.py # Auth logic
│   │   │   ├── services/
│   │   │   │   └── auth_service.py # Business logic
│   │   │   ├── schemas/
│   │   │   │   └── auth_dto.py # Pydantic models
│   │   │   ├── utils/
│   │   │   │   └── email_sender.py # Email utilities
│   │   │   └── security.py     # Security functions
│   │   ├── .env                # Environment variables
│   │   ├── requirements.txt    # Python dependencies
│   │   └── Dockerfile          # Docker configuration
│   │
│   ├── user_service/           # User management (Port 8002)
│   │   ├── app/
│   │   │   ├── main.py
│   │   │   ├── routes/
│   │   │   │   └── user_routes.py
│   │   │   ├── controllers/
│   │   │   │   └── user_controller.py
│   │   │   ├── services/
│   │   │   │   └── user_service.py
│   │   │   ├── schemas/
│   │   │   │   ├── user_dto.py
│   │   │   │   └── transaction_dto.py
│   │   │   └── scheduler/      # Background tasks
│   │   │       └── casso_poller.py # Payment polling
│   │   └── ...
│   │
│   ├── movie_service/          # Movie management (Port 8003)
│   │   ├── app/
│   │   │   ├── main.py
│   │   │   ├── routes/
│   │   │   │   ├── movie_routes.py
│   │   │   │   ├── admin_movie_routes.py
│   │   │   │   ├── comment_routes.py
│   │   │   │   └── featured_routes.py
│   │   │   ├── services/
│   │   │   │   ├── movie_service.py
│   │   │   │   ├── admin_movie_service.py
│   │   │   │   ├── comment_service.py
│   │   │   │   └── featured_service.py
│   │   │   └── ...
│   │   ├── add_movies.py       # Seed movies script
│   │   └── ...
│   │
│   ├── collection_service/     # Collections (Port 8005)
│   │   └── ...
│   │
│   ├── book_service/           # Books (Port 8004)
│   │   └── ...
│   │
│   ├── .env                    # Global backend config
│   ├── docker-compose.yml      # Development compose
│   ├── docker-compose.prod.yml # Production compose
│   ├── start_all_services.sh   # Auto-start script
│   ├── create_admin.py         # Create admin user
│   └── README.md               # Backend documentation
│
├── src/                        # Frontend source code
│   ├── components/             # React components
│   │   ├── admin/             # Admin components
│   │   │   └── ...
│   │   ├── common/            # Shared components
│   │   │   └── Modal/         # Modals
│   │   │       ├── CollectionModal.tsx
│   │   │       ├── DepositModal.tsx
│   │   │       ├── WalletModal.tsx
│   │   │       ├── AgeRestrictionModal.tsx
│   │   │       └── ...
│   │   ├── content/           # Content display
│   │   │   ├── ContentCard/
│   │   │   ├── ContentGrid/
│   │   │   ├── ContentFilters/
│   │   │   └── ContentSidebar/
│   │   ├── form/              # Form components
│   │   │   ├── LoginForm/
│   │   │   ├── RegisterForm/
│   │   │   └── ForgotPasswordForm/
│   │   ├── home/              # Home page components
│   │   │   ├── HeroBanner.tsx
│   │   │   ├── MovieInfo.tsx
│   │   │   └── MovieDetailHero.tsx
│   │   ├── layout/            # Layout components
│   │   │   ├── Header/
│   │   │   ├── Footer/
│   │   │   └── AdminLayout/
│   │   ├── search/            # Search components
│   │   │   └── SearchBox.tsx
│   │   └── providers/         # Context providers
│   │
│   ├── pages/                 # Page components
│   │   ├── admin/            # Admin pages
│   │   │   ├── Dashboard/
│   │   │   ├── UserManagement/
│   │   │   ├── ContentManagement/
│   │   │   ├── CommentModeration/
│   │   │   ├── Analytics/
│   │   │   └── Reports/
│   │   └── user/             # User pages
│   │       ├── Landing/
│   │       ├── Movies/
│   │       │   ├── MovieDetailPage.tsx
│   │       │   └── MovieWatch.tsx
│   │       ├── Premium/
│   │       └── Search/
│   │
│   ├── services/             # API services
│   │   ├── auth/
│   │   │   └── authService.ts
│   │   ├── content/
│   │   │   └── movieService.ts
│   │   ├── interaction/
│   │   │   └── collectionService.ts
│   │   ├── transaction/
│   │   │   └── transactionService.ts
│   │   └── admin/
│   │       ├── adminUserService.ts
│   │       ├── adminMovieService.ts
│   │       └── adminFeaturedService.ts
│   │
│   ├── hooks/                # Custom React hooks
│   │   ├── useAuth.ts
│   │   ├── useMovies.ts
│   │   └── useDocumentTitle.ts
│   │
│   ├── context/              # React contexts
│   │   └── AuthContext.tsx
│   │
│   ├── core/                 # Core utilities
│   │   ├── api/             # API utilities
│   │   │   ├── userServiceApi.ts
│   │   │   └── utils/
│   │   │       └── errors.ts
│   │   ├── security/        # Security utilities
│   │   │   └── ...
│   │   └── storage/         # Storage utilities
│   │       └── storageService.ts
│   │
│   ├── routes/              # Routing
│   │   ├── AppRoutes.tsx
│   │   └── PrivateRoute.tsx
│   │
│   ├── types/               # TypeScript types
│   │   ├── content.types.ts
│   │   └── user.types.ts
│   │
│   ├── config/              # Configuration
│   │   └── backend.config.ts
│   │
│   ├── styles/              # Global styles
│   │   └── *.css
│   │
│   ├── utils/               # Utilities
│   │   └── ageRating.ts
│   │
│   ├── App.tsx              # Root component
│   └── main.tsx             # Entry point
│
├── public/                   # Static assets
├── .env                      # Frontend environment variables
├── package.json              # Frontend dependencies
├── tsconfig.json             # TypeScript config
├── vite.config.ts            # Vite configuration
├── tailwind.config.js        # TailwindCSS config
├── eslint.config.js          # ESLint config
└── README.md                 # This file
```

## KIẾN TRÚC HỆ THỐNG

### Kiến trúc SOA (Service Oriented Architecture)

```
┌─────────────────────────────────────────────────────────────┐
│                    CLIENT LAYER                             │
│         (React SPA - Vercel Deployment)                     │
│   - React 19 + TypeScript + TailwindCSS                     │
│   - Zustand State Management                                │
│   - React Query Data Fetching                               │
└─────────────────────────────────────────────────────────────┘
                             │
                    HTTPS (REST APIs)
                             │
┌─────────────────────────────────────────────────────────────┐
│                    API GATEWAY LAYER                        │
│              (Direct Service Communication)                  │
│   - CORS Middleware                                         │
│   - Rate Limiting per Service                               │
│   - JWT Authentication                                      │
└─────────────────────────────────────────────────────────────┘
                             │
        ┌────────────────────┼────────────────────┐
        │                    │                    │
┌───────▼─────┐    ┌────────▼────────┐   ┌──────▼──────┐
│AUTH SERVICE │    │  USER SERVICE   │   │MOVIE SERVICE│
│  Port 8001  │    │   Port 8002     │   │  Port 8003  │
│             │    │                 │   │             │
│ - Login     │    │ - Profile       │   │ - CRUD      │
│ - Register  │    │ - Wallet        │   │ - Search    │
│ - OTP       │    │ - Premium       │   │ - Rating    │
│ - Password  │    │ - Transaction   │   │ - Comment   │
│   Reset     │    │ - Casso Webhook │   │ - Featured  │
└─────────────┘    └─────────────────┘   └─────────────┘
        │                    │                    │
        └────────────────────┼────────────────────┘
                             │
        ┌────────────────────┼────────────────────┐
        │                    │                    │
┌───────▼─────┐    ┌────────▼────────┐   ┌──────▼──────┐
│COLLECTION   │    │  BOOK SERVICE   │   │   SHARED    │
│  SERVICE    │    │   Port 8004     │   │ RESOURCES   │
│  Port 8005  │    │                 │   │             │
│             │    │ - Book CRUD     │   │ - MongoDB   │
│ - Create    │    │ - Categories    │   │ - Redis     │
│ - Add Items │    │ - Search        │   │ - SMTP      │
│ - Public/   │    │                 │   │             │
│   Private   │    │                 │   │             │
└─────────────┘    └─────────────────┘   └─────────────┘
        │                    │                    │
        └────────────────────┼────────────────────┘
                             │
┌─────────────────────────────▼───────────────────────────────┐
│                    DATA LAYER                               │
│                                                             │
│   ┌──────────────┐  ┌──────────────┐  ┌──────────────┐   │
│   │   MongoDB    │  │    Redis     │  │  File Storage│   │
│   │              │  │              │  │              │   │
│   │ - auth DB    │  │ - Sessions   │  │ - Images     │   │
│   │ - users DB   │  │ - Rate Limit │  │ - Videos     │   │
│   │ - movies DB  │  │ - Cache      │  │ - Documents  │   │
│   │ - books DB   │  │              │  │              │   │
│   │ - collections│  │              │  │              │   │
│   └──────────────┘  └──────────────┘  └──────────────┘   │
└─────────────────────────────────────────────────────────────┘
                             │
┌─────────────────────────────▼───────────────────────────────┐
│                EXTERNAL SERVICES                            │
│                                                             │
│   ┌──────────────┐  ┌──────────────┐  ┌──────────────┐   │
│   │   Casso API  │  │  SMTP Gmail  │  │   Railway    │   │
│   │  (Banking)   │  │   (Email)    │  │  (Hosting)   │   │
│   └──────────────┘  └──────────────┘  └──────────────┘   │
└─────────────────────────────────────────────────────────────┘
```

### Microservices Communication Flow

```
┌──────────┐                                    ┌──────────┐
│          │  1. Login Request                  │          │
│  Client  │───────────────────────────────────>│   Auth   │
│          │                                    │  Service │
│          │  2. JWT Token                      │          │
│          │<───────────────────────────────────│          │
└──────────┘                                    └──────────┘
     │                                                 │
     │ 3. Request with JWT                            │
     │────────────────────────────────────┐           │
     │                                    │           │
     ▼                                    ▼           ▼
┌──────────┐                       ┌──────────┐ ┌──────────┐
│  Movie   │  4. Verify JWT        │   User   │ │Collection│
│  Service │<──────────────────────│  Service │ │ Service  │
│          │                       │          │ │          │
│          │  5. User Data         │          │ │          │
│          │<──────────────────────│          │ │          │
│          │                       └──────────┘ └──────────┘
│          │  6. Response with Data
│          │───────────────────────────────────────>│
└──────────┘                                    └──────────┘
```

### Database Schema per Service

#### Auth Service Database (`finalsoa_auth`)

**Collection: users**

```javascript
{
  _id: ObjectId,
  username: String (unique),
  email: String (unique),
  password: String (hashed),
  is_verified: Boolean,
  created_at: DateTime,
  updated_at: DateTime
}
```

**Collection: otps**

```javascript
{
  _id: ObjectId,
  email: String,
  otp_code: String (6 digits),
  created_at: DateTime,
  expires_at: DateTime,
  purpose: String  // "registration", "password_reset"
}
```

**Collection: token_blacklist**

```javascript
{
  _id: ObjectId,
  token: String,
  blacklisted_at: DateTime,
  expires_at: DateTime
}
```

#### User Service Database (`finalsoa_user`)

**Collection: users**

```javascript
{
  _id: ObjectId,
  user_id: String (from auth service),
  full_name: String,
  date_of_birth: DateTime,
  phone: String,
  avatar_url: String,
  wallet_balance: Decimal,
  is_premium: Boolean,
  premium_expires_at: DateTime,
  is_banned: Boolean,
  ban_reason: String,
  created_at: DateTime,
  updated_at: DateTime
}
```

**Collection: transactions**

```javascript
{
  _id: ObjectId,
  user_id: String,
  type: String,  // "deposit", "purchase_premium", "refund"
  amount: Decimal,
  description: String,
  status: String,  // "pending", "completed", "failed"
  casso_transaction_id: String,
  created_at: DateTime,
  completed_at: DateTime
}
```

**Collection: premium_packages**

```javascript
{
  _id: ObjectId,
  name: String,
  duration_days: Integer,
  price: Decimal,
  features: Array,
  is_active: Boolean
}
```

#### Movie Service Database (`finalsoa_movie`)

**Collection: movies**

```javascript
{
  _id: ObjectId,
  title: String,
  description: String,
  poster_url: String,
  banner_url: String,
  video_url: String,
  trailer_url: String,
  genres: Array[String],
  release_year: Integer,
  duration_minutes: Integer,
  age_rating: String,  // "P", "13+", "16+", "18+"
  is_anime: Boolean,
  is_featured: Boolean,
  is_trending: Boolean,
  trending_timestamp: DateTime,
  views: Integer,
  total_likes: Integer,
  total_comments: Integer,
  average_rating: Decimal,
  rating_count: Integer,
  is_hidden: Boolean,
  created_at: DateTime,
  updated_at: DateTime
}
```

**Collection: comments**

```javascript
{
  _id: ObjectId,
  movie_id: ObjectId,
  user_id: String,
  username: String,
  content: String,
  is_approved: Boolean,
  created_at: DateTime,
  updated_at: DateTime
}
```

**Collection: ratings**

```javascript
{
  _id: ObjectId,
  movie_id: ObjectId,
  user_id: String,
  rating: Integer,  // 1-5
  created_at: DateTime,
  updated_at: DateTime
}
```

**Collection: likes**

```javascript
{
  _id: ObjectId,
  movie_id: ObjectId,
  user_id: String,
  created_at: DateTime
}
```

**Collection: featured_movies**

```javascript
{
  _id: ObjectId,
  movie_id: ObjectId,
  position: Integer,  // Display order
  is_active: Boolean,
  created_at: DateTime
}
```

#### Collection Service Database (`finalsoa_collection`)

**Collection: collections**

```javascript
{
  _id: ObjectId,
  user_id: String,
  name: String,
  description: String,
  is_public: Boolean,
  item_count: Integer,
  created_at: DateTime,
  updated_at: DateTime
}
```

**Collection: collection_items**

```javascript
{
  _id: ObjectId,
  collection_id: ObjectId,
  item_type: String,  // "movie", "book"
  item_id: ObjectId,
  added_at: DateTime
}
```

#### Book Service Database (`finalsoa_book`)

**Collection: books**

```javascript
{
  _id: ObjectId,
  title: String,
  author: String,
  description: String,
  cover_url: String,
  pdf_url: String,
  categories: Array[String],
  published_year: Integer,
  pages: Integer,
  views: Integer,
  total_likes: Integer,
  average_rating: Decimal,
  created_at: DateTime,
  updated_at: DateTime
}
```

### Security Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    SECURITY LAYERS                          │
│                                                             │
│  1. CORS Protection                                         │
│     - Whitelist allowed origins                             │
│     - Credentials handling                                  │
│                                                             │
│  2. Rate Limiting (SlowAPI)                                 │
│     - Per endpoint limits                                   │
│     - Per IP tracking                                       │
│     - OTP special limits (3 attempts/5min)                  │
│                                                             │
│  3. JWT Authentication                                      │
│     - Access token (30 minutes)                             │
│     - Refresh token (7 days)                                │
│     - Token blacklist on logout                             │
│                                                             │
│  4. Input Validation                                        │
│     - Pydantic models (Backend)                             │
│     - Zod schemas (Frontend)                                │
│     - React Hook Form validation                            │
│                                                             │
│  5. Password Security                                       │
│     - Passlib bcrypt hashing                                │
│     - Minimum complexity requirements                       │
│                                                             │
│  6. Age Verification                                        │
│     - Date of birth validation                              │
│     - Age-restricted content blocking                       │
│                                                             │
│  7. Transaction Safety                                      │
│     - MongoDB transactions                                  │
│     - Idempotency keys                                      │
│     - Webhook signature verification                        │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## DATABASE SCHEMA

Xem chi tiết trong phần [Kiến trúc hệ thống](#database-schema-per-service) ở trên.

## API ENDPOINTS

### Auth Service (Port 8001)

#### POST `/api/auth/register`

Đăng ký tài khoản mới

**Request:**

```json
{
  "username": "user123",
  "email": "user@example.com",
  "password": "SecurePass123!"
}
```

**Response:**

```json
{
  "success": true,
  "message": "Registration successful. Please check your email to verify your account."
}
```

#### POST `/api/auth/verify-email`

Xác thực email với OTP

**Request:**

```json
{
  "email": "user@example.com",
  "otp_code": "123456"
}
```

#### POST `/api/auth/login`

Đăng nhập

**Request:**

```json
{
  "username": "user123",
  "password": "SecurePass123!"
}
```

**Response:**

```json
{
  "access_token": "eyJhbGciOiJIUzI1NiIs...",
  "refresh_token": "eyJhbGciOiJIUzI1NiIs...",
  "token_type": "bearer",
  "user": {
    "user_id": "507f1f77bcf86cd799439011",
    "username": "user123",
    "email": "user@example.com"
  }
}
```

#### POST `/api/auth/logout`

Đăng xuất (blacklist token)

**Headers:** `Authorization: Bearer <token>`

#### POST `/api/auth/forgot-password`

Yêu cầu reset mật khẩu

**Request:**

```json
{
  "email": "user@example.com"
}
```

#### POST `/api/auth/reset-password`

Reset mật khẩu với OTP

**Request:**

```json
{
  "email": "user@example.com",
  "otp_code": "123456",
  "new_password": "NewSecurePass123!"
}
```

### User Service (Port 8002)

#### GET `/api/user/profile`

Lấy thông tin profile

**Headers:** `Authorization: Bearer <token>`

**Response:**

```json
{
  "user_id": "507f1f77bcf86cd799439011",
  "full_name": "Nguyen Van A",
  "email": "user@example.com",
  "date_of_birth": "2000-01-01",
  "phone": "0123456789",
  "avatar_url": "https://...",
  "wallet_balance": 100000,
  "is_premium": true,
  "premium_expires_at": "2025-12-31T23:59:59Z"
}
```

#### PUT `/api/user/profile`

Cập nhật profile

#### GET `/api/user/wallet`

Lấy thông tin ví

#### GET `/api/user/transactions`

Lấy lịch sử giao dịch

**Query params:**

- `skip`: số record bỏ qua (pagination)
- `limit`: số record tối đa

#### POST `/api/user/purchase-premium`

Mua Premium

**Request:**

```json
{
  "package_id": "507f1f77bcf86cd799439011"
}
```

#### POST `/api/user/casso-webhook`

Webhook xử lý thanh toán tự động (Casso)

### Movie Service (Port 8003)

#### GET `/api/movies`

Lấy danh sách phim

**Query params:**

- `skip`, `limit`: pagination
- `search`: tìm kiếm theo tên
- `genre`: lọc theo thể loại
- `age_rating`: lọc theo độ tuổi
- `year`: lọc theo năm
- `is_anime`: lọc anime
- `is_trending`: phim trending
- `sort_by`: sắp xếp (views, rating, created_at)

#### GET `/api/movies/{movie_id}`

Lấy chi tiết phim

#### POST `/api/movies/{movie_id}/like`

Like phim

**Headers:** `Authorization: Bearer <token>`

#### POST `/api/movies/{movie_id}/rate`

Đánh giá phim

**Request:**

```json
{
  "rating": 5
}
```

#### GET `/api/movies/{movie_id}/comments`

Lấy bình luận

#### POST `/api/movies/{movie_id}/comments`

Thêm bình luận

**Request:**

```json
{
  "content": "Great movie!"
}
```

#### GET `/api/movies/featured`

Lấy phim featured

#### GET `/api/movies/search`

Tìm kiếm nâng cao

**Query params:**

- `query`: từ khóa
- `filters`: JSON object với các filter

### Admin Movie Routes (Port 8003)

#### POST `/api/admin/movies`

Tạo phim mới (Admin only)

#### PUT `/api/admin/movies/{movie_id}`

Cập nhật phim

#### DELETE `/api/admin/movies/{movie_id}`

Xóa phim

#### PUT `/api/admin/movies/{movie_id}/hide`

Ẩn/hiện phim

#### GET `/api/admin/comments`

Lấy danh sách comment cần kiểm duyệt

#### PUT `/api/admin/comments/{comment_id}/approve`

Duyệt comment

### Collection Service (Port 8005)

#### GET `/api/collections`

Lấy danh sách collections của user

#### POST `/api/collections`

Tạo collection mới

**Request:**

```json
{
  "name": "My Favorites",
  "description": "Movies I love",
  "is_public": true
}
```

#### POST `/api/collections/{collection_id}/items`

Thêm item vào collection

**Request:**

```json
{
  "item_type": "movie",
  "item_id": "507f1f77bcf86cd799439011"
}
```

#### DELETE `/api/collections/{collection_id}/items/{item_id}`

Xóa item khỏi collection

### Book Service (Port 8004)

#### GET `/api/books`

Lấy danh sách sách

#### GET `/api/books/{book_id}`

Chi tiết sách

#### POST `/api/books`

Tạo sách mới (Admin)

---

**📚 Full API Documentation:**

Mỗi service có Swagger UI docs:

- Auth: http://localhost:8001/docs
- User: http://localhost:8002/docs
- Movie: http://localhost:8003/docs
- Collection: http://localhost:8005/docs
- Book: http://localhost:8004/docs

## CẤU HÌNH MÔI TRƯỜNG

### Environment Variables

Chi tiết xem trong phần [Cài đặt - Bước 5](#5-cấu-hình-environment-variables)

### CORS Configuration

Tất cả services đều config CORS cho phép:

```python
allow_origins = [
    "http://localhost:5173",      # Vite dev server
    "http://localhost:3000",      # Alternative port
    "https://final-soa.vercel.app"  # Production
]
```

### Rate Limiting

**General endpoints:** 60 requests/minute
**OTP endpoints:** 3 requests/5 minutes
**Login endpoint:** 5 requests/minute

### JWT Configuration

- **Access Token**: 30 minutes
- **Refresh Token**: 7 days
- **Algorithm**: HS256
- **Secret**: Phải giống nhau ở tất cả services

### Email Configuration (SMTP)

Dùng Gmail với App Password:

1. Enable 2-Factor Authentication
2. Tạo App Password tại: https://myaccount.google.com/apppasswords
3. Dùng App Password thay vì password thường

### Casso API Configuration

Để tích hợp thanh toán tự động:

1. Đăng ký tài khoản tại [casso.vn](https://casso.vn)
2. Lấy API Key và Webhook Secret
3. Cấu hình webhook URL trong Casso dashboard
4. Cập nhật vào `.env` của user_service

## TÀI KHOẢN DEMO

### Admin Account

```
Username: admin
Password: admin123
Email: admin@example.com
```

**Quyền hạn:**

- Quản lý tất cả phim
- Quản lý tất cả người dùng
- Kiểm duyệt bình luận
- Xem analytics và reports
- Cấu hình featured movies

### User Account 1 (Premium)

```
Username: user1
Password: user123
Email: user1@example.com
Wallet Balance: 500,000 VNĐ
Premium: Active (expires 2026-12-31)
```

### User Account 2 (Regular)

```
Username: user2
Password: user123
Email: user2@example.com
Wallet Balance: 100,000 VNĐ
Premium: None
```

### User Account 3 (Under 18)

```
Username: teen_user
Password: user123
Email: teen@example.com
Date of Birth: 2010-01-01
Wallet Balance: 50,000 VNĐ
```

**Lưu ý:** User dưới 18 tuổi sẽ bị chặn xem phim có age rating 18+

## HƯỚNG DẪN SỬ DỤNG

### Đăng ký và Đăng nhập

1. Truy cập `http://localhost:5173`
2. Click "Đăng ký" ở góc trên phải
3. Điền thông tin: username, email, password
4. Nhận OTP qua email
5. Nhập OTP để xác thực
6. Đăng nhập với username và password

### Xem Phim

1. Sau khi đăng nhập, browse danh sách phim
2. Click vào phim để xem chi tiết
3. Click "Xem phim" để streaming
4. Đánh giá và comment phim

### Nạp Tiền Vào Ví

#### Cách 1: Chuyển khoản ngân hàng (Tự động)

1. Vào "Ví của tôi"
2. Click "Nạp tiền"
3. Quét QR Code hoặc chuyển khoản thủ công
4. Nội dung chuyển khoản: `NAP [user_id]`
5. Sau vài giây, số dư tự động cập nhật

#### Cách 2: Admin cộng thủ công

Admin có thể adjust wallet cho user trong User Management.

### Mua Premium

1. Vào "Premium"
2. Chọn gói Premium (1 tháng / 3 tháng / 1 năm)
3. Click "Mua ngay"
4. Xác nhận thanh toán (từ ví)
5. Premium được kích hoạt ngay lập tức

### Tạo Bộ Sưu Tập

1. Vào "Bộ sưu tập"
2. Click "Tạo bộ sưu tập mới"
3. Đặt tên và mô tả
4. Chọn Public/Private
5. Thêm phim vào collection từ trang chi tiết phim

### Admin - Quản lý Phim

1. Đăng nhập với tài khoản admin
2. Vào Admin Dashboard
3. Content Management → Movies
4. CRUD operations: Tạo, Sửa, Xóa, Ẩn/Hiện phim
5. Set Featured Movies cho homepage
6. Kiểm duyệt comments

### Admin - Quản lý User

1. Admin Dashboard → User Management
2. Xem danh sách users
3. View chi tiết user
4. Ban/Unban users
5. Adjust wallet balance
6. Grant/Revoke Premium

## TROUBLESHOOTING

### Lỗi thường gặp

#### 1. Port already in use

```bash
Error: Port 8001 is already in use
```

**Giải pháp:**

```bash
# Kill process trên port cụ thể
lsof -ti:8001 | xargs kill -9

# Hoặc kill tất cả backend ports
lsof -ti:8001,8002,8003,8004,8005 | xargs kill -9
```

#### 2. MongoDB connection failed

```bash
pymongo.errors.ServerSelectionTimeoutError
```

**Giải pháp:**

- ✅ Kiểm tra MongoDB đang chạy: `mongosh` hoặc MongoDB Compass
- ✅ Kiểm tra MONGODB_URL trong .env
- ✅ Nếu dùng Atlas, check IP whitelist
- ✅ Test connection: `mongosh "mongodb://localhost:27017"`

#### 3. CORS Error

```bash
Access to XMLHttpRequest blocked by CORS policy
```

**Giải pháp:**

- ✅ Kiểm tra frontend URL trong backend CORS config
- ✅ Đảm bảo `allow_credentials=True`
- ✅ Check frontend đúng port (5173)

#### 4. JWT Token Invalid

```bash
401 Unauthorized - Invalid token
```

**Giải pháp:**

- ✅ Check JWT_SECRET_KEY giống nhau ở tất cả services
- ✅ Logout và login lại
- ✅ Clear localStorage: `localStorage.clear()`

#### 5. OTP không nhận được email

**Checklist:**

- ✅ Check spam folder
- ✅ Verify SMTP credentials
- ✅ Dùng App Password, không phải password Gmail thường
- ✅ Enable "Less secure app access" (nếu cần)
- ✅ Check backend logs

#### 6. Rate Limit Exceeded

```bash
429 Too Many Requests
```

**Giải pháp:**

- Đợi 1-5 phút (tùy endpoint)
- Nếu development: Tạm disable rate limiting trong code
- Nếu production: Implement retry logic với exponential backoff

#### 7. Payment webhook không hoạt động

**Checklist:**

- ✅ Check Casso webhook URL đúng chưa
- ✅ Verify webhook secret
- ✅ Check ngrok hoặc public URL accessible
- ✅ Test webhook với Casso test tool
- ✅ Check logs: `tail -f /tmp/user_service.log`

#### 8. Video không play được

**Giải pháp:**

- ✅ Check video_url có accessible không
- ✅ Check CORS cho video CDN
- ✅ Thử video URL trực tiếp trong browser
- ✅ Check Premium status (nếu required)
- ✅ Check age rating permissions

#### 9. Frontend build failed

```bash
npm ERR! code ERESOLVE
```

**Giải pháp:**

```bash
# Clear cache
rm -rf node_modules package-lock.json

# Reinstall
npm install

# Hoặc force
npm install --force
```

#### 10. Docker containers không start

```bash
docker-compose up failed
```

**Giải pháp:**

```bash
# Check logs
docker-compose logs

# Rebuild images
docker-compose down
docker-compose build --no-cache
docker-compose up -d

# Check ports
docker ps
```

### Debug Tips

1. **Check Service Health:**

```bash
curl http://localhost:8001/health
curl http://localhost:8002/health
curl http://localhost:8003/health
```

2. **View Logs:**

```bash
# Script-based services
tail -f /tmp/auth_service.log
tail -f /tmp/user_service.log

# Docker services
docker-compose logs -f auth_service
docker-compose logs -f user_service
```

3. **MongoDB Queries:**

```bash
mongosh
use finalsoa_movie
db.movies.find().limit(5).pretty()
db.users.countDocuments()
```

4. **Network Debugging:**

```bash
# Check all listening ports
netstat -an | grep LISTEN

# Check specific service
lsof -i:8003
```

5. **Frontend Console:**

- F12 → Console tab
- F12 → Network tab (check API requests)
- React DevTools extension

## BEST PRACTICES ĐÃ ÁP DỤNG

### Architecture

- ✅ Microservices architecture với clear boundaries
- ✅ Separation of concerns (Routes → Controllers → Services)
- ✅ Database per service pattern
- ✅ API Gateway pattern (mỗi service độc lập)
- ✅ Event-driven với webhooks

### Security

- ✅ JWT authentication với refresh tokens
- ✅ Password hashing với bcrypt
- ✅ Rate limiting per endpoint
- ✅ Input validation (Pydantic + Zod)
- ✅ CORS configuration
- ✅ Token blacklist on logout
- ✅ Age verification
- ✅ Webhook signature verification

### Performance

- ✅ MongoDB indexing
- ✅ Redis caching layer
- ✅ React Query data caching
- ✅ Lazy loading components
- ✅ Infinite scroll pagination
- ✅ Image optimization
- ✅ Debounced search

### Code Quality

- ✅ TypeScript type safety
- ✅ ESLint + TypeScript ESLint
- ✅ Pydantic models validation
- ✅ Error handling với try-catch
- ✅ Consistent naming conventions
- ✅ Code splitting
- ✅ Component composition

### DevOps

- ✅ Docker containerization
- ✅ Docker Compose orchestration
- ✅ Environment variables
- ✅ Health check endpoints
- ✅ Structured logging
- ✅ CI/CD ready

## DEPLOYMENT

### Frontend (Vercel)

1. Push code lên GitHub
2. Import project vào Vercel
3. Configure environment variables
4. Deploy automatically

**Production URL:** https://final-soa.vercel.app

### Backend (Railway)

Mỗi service deploy riêng:

1. Connect GitHub repository
2. Create new service
3. Select Dockerfile
4. Configure environment variables
5. Deploy

**Production URLs:**

- Auth: https://finalsoa-production-7873.up.railway.app
- User: https://finalsoa-production-965a.up.railway.app
- Movie: https://finalsoa-production.up.railway.app

### Database (MongoDB Atlas)

1. Create cluster tại [mongodb.com/cloud/atlas](https://mongodb.com/cloud/atlas)
2. Create database user
3. Whitelist IP (0.0.0.0/0 cho development)
4. Get connection string
5. Update MONGODB_URL trong .env

## LIÊN HỆ & HỖ TRỢ

- **Repository**: [https://github.com/JasonNguyen31/finalsoa](https://github.com/JasonNguyen31/ONLINE-MOVIE-WATCHING-AND-MANAGEMENT-SYSTEM/edit/main/README.md)
- **Email**: 521h0185@student.tdtu.edu.vn

## THÀNH VIÊN NHÓM

- **Nguyễn Hoàng Việt** - 521H0185 - Team Lead & Frontend Developer
- **Nguyễn Ngọc Nghĩa** - 523H0062 - Backend Developer
- **Nguyễn Ngọc Trinh Nghi** - 523H0061 - Backend Developer

## LICENSE

Copyright © 2025 Ton Duc Thang University. All rights reserved.

Developed by TDT Software Team for educational purposes.

# ClientFlow — MongoDB Atlas + Mongoose

This version uses MongoDB Atlas + Mongoose instead of the old local JSON database.

## Setup

1. Create a MongoDB Atlas cluster.
2. Create a database user.
3. In Atlas Network Access, allow your development IP. For temporary local testing you can use `0.0.0.0/0`, but restrict it for production.
4. Choose Connect → Drivers and copy your MongoDB URI.
5. In VS Code:

```bash
npm install
```

6. Copy `.env.example` to `.env` and put your Atlas URI in `MONGODB_URI`:

```env
PORT=4000
SESSION_SECRET=your-long-random-secret
MONGODB_URI=mongodb+srv://USERNAME:PASSWORD@cluster.mongodb.net/clientflow?retryWrites=true&w=majority
ADMIN_PASSWORD=YourStrongAdminPassword123!
```

If the MongoDB password contains special URL characters, URL-encode it.

7. Start:

```bash
npm run dev
```

Client: http://localhost:5173/client
Admin: http://localhost:5173/login/admin

## Local accounts

Admin: `admin@clientflow.local` / `ChangeMe123!`

Client: `ayesha@urbancart.pk` / `Client@123`

If `ADMIN_PASSWORD` is set before the first database seed, that value becomes the initial admin password.

## MongoDB collections

`users`, `clients`, `tasks`, `documents`, `messages`, `automations`

The first start automatically seeds portfolio data when the database has no users.

## Important

The previous `server/data/db.json` database has been removed. `MONGODB_URI` is now required.

Uploaded file metadata is stored in MongoDB while files are stored locally in `server/uploads/`. For production, use persistent cloud storage such as S3/Cloudinary/Supabase Storage.

For production also use HTTPS, a persistent session store, restricted Atlas access, environment secrets, rate limiting, CSRF protection, validation, and a real SMTP provider.

## Client self-registration

New clients can now create their own ClientFlow account at:

`http://localhost:5173/register`

Registration creates a MongoDB client profile, a secure client user account, a unique Client ID, six onboarding tasks, and a welcome message. If SMTP is configured, the welcome email is also sent automatically.

After registration, the client is signed in automatically and redirected to `/client`. The new client then appears in the Admin Portal client list.

Required registration fields:
- Full name
- Company / business name
- Email address
- Phone number
- Password (minimum 8 characters)

Admin registration is intentionally not public. Admin accounts remain controlled by the seeded/admin workflow.

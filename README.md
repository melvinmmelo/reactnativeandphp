# React Native Authentication App

A mobile authentication application built with React Native and Expo, demonstrating user registration, login, and a protected home screen with integration to a PHP backend API.

## Features

- User Registration with validation
- User Login with email and password
- Protected Home Screen with user information
- Form validation and error handling
- Loading states and user feedback
- Cross-platform support (iOS, Android, Web)

## Tech Stack

- **React Native** 0.81.4
- **React** 19.1.0
- **Expo** ~54.0.13
- **React Navigation** 7.x (Native Stack)
- **PHP Backend** (separate repository)

## Prerequisites

Before running this app, ensure you have:

- [Node.js](https://nodejs.org/) (v16 or higher)
- [Expo CLI](https://docs.expo.dev/get-started/installation/)
- Expo Go app on your mobile device, or
- Android Studio (for Android Emulator), or
- Xcode (for iOS Simulator, macOS only)
- PHP backend server running locally

## Installation

1. Clone the repository:
```bash
git clone <repository-url>
cd styling
```

2. Install dependencies:
```bash
npm install
```

3. Configure the API endpoint in `services/authService.js`:
```javascript
// For Android Emulator
const API_BASE_URL = "http://10.0.2.2/phpauthsystem/api";

// For iOS Simulator
const API_BASE_URL = "http://localhost/phpauthsystem/api";

// For Physical Device (replace with your computer's IP)
const API_BASE_URL = "http://192.168.1.XXX/phpauthsystem/api";
```

## Running the App

Start the development server:
```bash
npx expo start
```

Or clear cache and start:
```bash
npx expo start --clear
```

### Run on Different Platforms

**Android:**
```bash
npm run android
```
Press `a` in the terminal after running `npx expo start`

**iOS:**
```bash
npm run ios
```
Press `i` in the terminal after running `npx expo start`

**Web:**
```bash
npm run web
```
Press `w` in the terminal after running `npx expo start`

**Physical Device:**
1. Install Expo Go from App Store or Google Play
2. Scan the QR code displayed in the terminal

## Project Structure

```
styling/
├── screens/
│   ├── LoginScreen.js      # User login interface
│   ├── RegisterScreen.js   # User registration interface
│   └── HomeScreen.js        # Protected dashboard
├── services/
│   └── authService.js      # API integration layer
├── App.js                  # Main navigation setup
├── index.js                # Entry point
└── package.json            # Dependencies
```

## Screen Flow

```
Login Screen (Initial)
    ↓
    ├─→ Register Screen → (Success) → Back to Login
    │
    └─→ (Login Success) → Home Screen → Logout → Back to Login
```

## API Endpoints

The app expects the following PHP API endpoints:

### POST /api/login.php
**Request:**
```json
{
  "email": "user@example.com",
  "password": "password123"
}
```

**Response:**
```json
{
  "success": true,
  "user": {
    "id": 1,
    "username": "John Doe",
    "email": "user@example.com"
  }
}
```

### POST /api/register.php
**Request:**
```json
{
  "username": "John Doe",
  "email": "user@example.com",
  "password": "password123"
}
```

**Response:**
```json
{
  "success": true,
  "message": "Registration successful"
}
```

## Features & Validation

### Registration
- All fields required
- Password must be at least 6 characters
- Password confirmation must match
- Email format validation (backend)

### Login
- Email and password required
- Displays loading indicator during authentication
- Shows error alerts for failed attempts

### Home Screen
- Displays user information
- Dashboard with placeholder stats
- Logout functionality

## Troubleshooting

### API Connection Issues

**Problem:** Network request failed or connection timeout

**Solutions:**
1. Verify the PHP backend server is running
2. Check `API_BASE_URL` in `services/authService.js` matches your environment
3. For physical devices, ensure your phone and computer are on the same network
4. Disable firewall temporarily to test connectivity
5. Test API endpoints directly with Postman or cURL

### Platform-Specific Issues

**Android Emulator:**
- Use `10.0.2.2` instead of `localhost`
- Enable network access in emulator settings

**iOS Simulator:**
- Use `localhost` or `127.0.0.1`
- Check that PHP server allows local connections

**Physical Device:**
- Find your computer's local IP address
- Windows: `ipconfig` in Command Prompt
- macOS/Linux: `ifconfig` in Terminal
- Update `API_BASE_URL` with this IP

### Cache Issues

If experiencing unexpected behavior:
```bash
npx expo start --clear
```

## Development

### Adding New Screens

1. Create screen component in `screens/` directory
2. Import in `App.js`
3. Add to Stack Navigator:
```javascript
<Stack.Screen name="NewScreen" component={NewScreen} />
```

### Navigating Between Screens

```javascript
// Simple navigation
navigation.navigate('ScreenName');

// Navigation with parameters
navigation.navigate('ScreenName', { param1: 'value' });

// Access parameters
const { param1 } = route.params;
```

### Styling

The app uses React Native's StyleSheet API:
```javascript
const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#f5f5f5',
  },
});
```

## Educational Resources

- `docu.md` - Comprehensive documentation for understanding the codebase
- `teaching_guide.md` - Lesson plans and exercises for instructors
- `CLAUDE.md` - Development guide for Claude Code

## License

This project is created for educational purposes.

## Support

For issues or questions:
1. Check console logs for debugging information
2. Review the `docu.md` file for detailed explanations
3. Ensure PHP backend is properly configured
4. Verify network connectivity between app and API

# React Native Authentication App

## Overview
This guide will help you understand the React Native authentication application. The app demonstrates user registration, login, and a home dashboard using React Navigation and a PHP backend API.

## Table of Contents
1. [Project Structure](#project-structure)
2. [Technologies Used](#technologies-used)
3. [App Architecture](#app-architecture)
4. [Screen-by-Screen Breakdown](#screen-by-screen-breakdown)
5. [Styling Concepts](#styling-concepts)
6. [API Integration](#api-integration)
7. [Navigation Flow](#navigation-flow)
8. [Key Learning Points](#key-learning-points)
9. [Running the App](#running-the-app)

---

## Project Structure

```
styling/
├── screens/
│   ├── LoginScreen.js       # Login interface
│   ├── RegisterScreen.js    # User registration interface
│   └── HomeScreen.js         # Dashboard after login
├── services/
│   └── authService.js       # API communication functions
├── App.js                   # Main app entry with navigation
└── package.json             # Project dependencies
```

---

## Technologies Used

### Core Technologies
- **React Native 0.81.4** - Framework for building native mobile apps
- **React 19.1.0** - JavaScript library for UI
- **Expo ~54.0.13** - Development platform for React Native

### Navigation
- **@react-navigation/native** - Navigation library
- **@react-navigation/native-stack** - Stack navigation for screens

### Key Concepts Demonstrated
- Component-based architecture
- State management with hooks
- Async/await for API calls
- Form validation
- Loading states
- Platform-specific code
- StyleSheet API

---

## App Architecture

### Navigation Structure
```
Stack Navigator
├── Login Screen (Initial)
├── Register Screen
└── Home Screen (Protected)
```

### Data Flow
```
User Input → Screen Component → authService → PHP API
                ↓
         Update State
                ↓
      Navigation/UI Update
```

---

## Screen-by-Screen Breakdown

### 1. Login Screen (`screens/LoginScreen.js`)

**Purpose**: Allow existing users to log into the app

**Key Components**:
- `KeyboardAvoidingView` - Adjusts UI when keyboard appears
- `TextInput` - Email and password fields
- `TouchableOpacity` - Button component
- `ActivityIndicator` - Loading spinner

**State Management**:
```javascript
const [email, setEmail] = useState('');          // Stores email input
const [password, setPassword] = useState('');     // Stores password input
const [loading, setLoading] = useState(false);    // Tracks loading state
```

**Key Functions**:

**`handleLogin()`** - Main login logic
```javascript
1. Validates that both fields are filled
2. Sets loading state to true
3. Calls the login API service
4. If successful: navigates to Home screen with user data
5. If failed: shows error alert
6. Finally: sets loading to false
```

**Important Props & Features**:
- `autoCapitalize="none"` - Prevents auto-capitalization for email
- `keyboardType="email-address"` - Shows appropriate keyboard
- `secureTextEntry` - Hides password text
- `editable={!loading}` - Disables inputs while loading
- `disabled={loading}` - Disables button while loading

**Styling Highlights**:
- Flexbox layout for centering
- Conditional styling for disabled state
- Border and shadow effects
- Responsive padding

---

### 2. Register Screen (`screens/RegisterScreen.js`)

**Purpose**: Allow new users to create an account

**Additional Components**:
- `ScrollView` - Allows scrolling for longer forms

**State Management**:
```javascript
const [name, setName] = useState('');
const [email, setEmail] = useState('');
const [password, setPassword] = useState('');
const [confirmPassword, setConfirmPassword] = useState('');
const [loading, setLoading] = useState(false);
```

**Key Functions**:

**`handleRegister()`** - Registration logic with validation
```javascript
1. Validates all fields are filled
2. Checks if passwords match
3. Validates password length (minimum 6 characters)
4. Calls register API service
5. If successful: shows success alert and navigates to Login
6. If failed: shows error alert
```

**Validation Rules**:
- All fields required
- Passwords must match
- Password minimum 6 characters
- Email format (handled by backend)

**User Experience Features**:
- Success alert with callback navigation
- Detailed error messages
- ScrollView for keyboard management
- Form resets on successful registration

---

### 3. Home Screen (`screens/HomeScreen.js`)

**Purpose**: Main dashboard after successful login

**Features**:
- Welcome message with user email
- Dashboard card with welcome text
- Quick stats display (placeholder data)
- Logout functionality

**Route Parameters**:
```javascript
const user = route?.params?.user;  // Receives user data from login
```

**UI Structure**:
```
Header (Blue background)
  ├── Welcome Text
  └── User Email

Content (Scrollable)
  ├── Dashboard Card
  ├── Stats Card
  │   ├── Tasks (0)
  │   ├── Projects (0)
  │   └── Messages (0)
  └── Logout Button
```

**Styling Techniques**:
- Colored header with paddingTop for status bar
- Card design with shadows
- Flexbox for horizontal stats layout
- Different button colors for different actions

---

## Styling Concepts

### 1. StyleSheet API
React Native uses JavaScript objects for styling, not CSS.

```javascript
const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#f5f5f5',
  },
});
```

**Benefits**:
- Better performance (styles are compiled)
- Type checking and validation
- Autocomplete in editors

### 2. Flexbox Layout
Default layout system in React Native.

**Key Properties Demonstrated**:
```javascript
flex: 1                    // Takes all available space
flexDirection: 'row'       // Horizontal layout
justifyContent: 'center'   // Center along main axis
alignItems: 'center'       // Center along cross axis
```

### 3. Common Style Properties

**Spacing**:
- `padding` / `paddingHorizontal` / `paddingVertical`
- `margin` / `marginBottom` / `marginTop`

**Typography**:
- `fontSize` - Size in logical pixels
- `fontWeight` - '400', '600', 'bold'
- `textAlign` - 'center', 'left', 'right'
- `color` - Hex or color name

**Visual Effects**:
- `borderRadius` - Rounded corners
- `borderWidth` / `borderColor` - Borders
- `backgroundColor` - Background color
- `opacity` - Transparency (0-1)

### 4. Shadows

**iOS Shadows**:
```javascript
shadowColor: '#000',
shadowOffset: { width: 0, height: 2 },
shadowOpacity: 0.1,
shadowRadius: 3,
```

**Android Elevation**:
```javascript
elevation: 3,  // Creates shadow on Android
```

### 5. Conditional Styling
```javascript
style={[styles.button, loading && styles.buttonDisabled]}
```
Combines base style with conditional style when loading is true.

### 6. Platform-Specific Code
```javascript
behavior={Platform.OS === 'ios' ? 'padding' : 'height'}
```
Different behavior for iOS and Android.

---

## API Integration

### Service Architecture (`services/authService.js`)

**Purpose**: Centralized API communication

**Base URL Configuration**:
```javascript
const API_BASE_URL = "http://10.0.2.2/phpauthsystem/api";
```

**Important**: Different addresses for different environments:
- Android Emulator: `10.0.2.2`
- iOS Simulator: `localhost`
- Physical Device: Your computer's local IP (e.g., `192.168.1.xxx`)

### Login Function

```javascript
export const login = async (email, password) => {
  // 1. Make POST request to API
  const response = await fetch(`${API_BASE_URL}/login.php`, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'Accept': 'application/json',
    },
    body: JSON.stringify({ email, password }),
  });

  // 2. Parse response
  const responseText = await response.text();
  const data = JSON.parse(responseText);

  // 3. Return data or throw error
  if (!response.ok) {
    throw new Error(data.message || 'Login failed');
  }

  return data;
};
```

**Key Concepts**:
- `async/await` - Modern JavaScript async handling
- `fetch()` - API for HTTP requests
- `JSON.stringify()` - Convert JS object to JSON
- `JSON.parse()` - Convert JSON to JS object
- Error handling with try/catch

### Register Function

Similar structure to login, but sends username, email, and password:
```javascript
body: JSON.stringify({
  username,
  email,
  password,
})
```

**Debugging Features**:
- Console logs for request/response tracking
- Raw response text logging
- JSON parsing error handling
- Detailed error messages

---

## Navigation Flow

### Setup in App.js

```javascript
<NavigationContainer>
  <Stack.Navigator
    initialRouteName="Login"
    screenOptions={{ headerShown: false }}
  >
    <Stack.Screen name="Login" component={LoginScreen} />
    <Stack.Screen name="Register" component={RegisterScreen} />
    <Stack.Screen name="Home" component={HomeScreen} />
  </Stack.Navigator>
</NavigationContainer>
```

### Navigation Object
Every screen component receives a `navigation` prop with methods:

**Navigate to screen**:
```javascript
navigation.navigate('Home', { user: userData })
```

**Navigate with parameters**:
```javascript
navigation.navigate('Home', { user: response.user })
```

**Access route parameters**:
```javascript
const user = route?.params?.user;
```

### Navigation Flow Diagram

```
┌─────────────┐
│   Login     │
│   Screen    │
└──────┬──────┘
       │
       ├──→ Register ──→ (Success) ──→ Back to Login
       │
       └──→ (Success) ──→ Home Screen ──→ Logout ──→ Back to Login
```

---

## Key Learning Points

### 1. React Hooks Usage

**useState** - Managing component state
```javascript
const [value, setValue] = useState(initialValue);
```

**Common Pattern**: Input handling
```javascript
<TextInput
  value={email}
  onChangeText={setEmail}
/>
```

### 2. Async Operations

**Pattern**: Loading states with try/catch/finally
```javascript
setLoading(true);
try {
  const result = await apiCall();
  // Handle success
} catch (error) {
  // Handle error
} finally {
  setLoading(false);  // Always runs
}
```

### 3. Form Validation

**Client-Side Validation Checklist**:
- Check required fields
- Validate format (email, password strength)
- Compare fields (password confirmation)
- Provide clear error messages

### 4. User Feedback

**Visual Feedback Methods**:
- Loading indicators (ActivityIndicator)
- Disabled states (grayed out buttons)
- Alerts for errors and success
- Navigation on success

### 5. Component Composition

**Building Blocks**:
- Container components (View, ScrollView)
- Input components (TextInput)
- Interactive components (TouchableOpacity)
- Feedback components (ActivityIndicator, Alert)

---

## Styling Deep Dive

### Color Palette Used

```javascript
Primary Blue:    '#007AFF'    // Buttons, header
Error Red:       '#FF3B30'    // Logout, errors
Background:      '#f5f5f5'    // Screen background
Card White:      '#fff'       // Card backgrounds
Text Dark:       '#333'       // Primary text
Text Gray:       '#666'       // Secondary text
Border Gray:     '#ddd'       // Input borders
Disabled Gray:   '#999'       // Disabled buttons
```

### Common Patterns

**Centered Container**:
```javascript
container: {
  flex: 1,
  justifyContent: 'center',
  paddingHorizontal: 30,
}
```

**Card Design**:
```javascript
card: {
  backgroundColor: '#fff',
  borderRadius: 12,
  padding: 20,
  shadowColor: '#000',
  shadowOffset: { width: 0, height: 2 },
  shadowOpacity: 0.1,
  shadowRadius: 3,
  elevation: 3,
}
```

**Primary Button**:
```javascript
button: {
  backgroundColor: '#007AFF',
  paddingVertical: 15,
  borderRadius: 8,
}
buttonText: {
  color: '#fff',
  textAlign: 'center',
  fontSize: 16,
  fontWeight: '600',
}
```

**Text Input**:
```javascript
input: {
  backgroundColor: '#fff',
  paddingHorizontal: 15,
  paddingVertical: 12,
  borderRadius: 8,
  fontSize: 16,
  borderWidth: 1,
  borderColor: '#ddd',
}
```

---

## Running the App

### Prerequisites
1. Node.js installed
2. Expo CLI installed globally
3. Expo Go app on your phone OR Android Studio/Xcode for emulator
4. PHP backend running locally

### Setup Steps

**1. Install dependencies**:
```bash
npm install
```

**2. Configure API URL** in `services/authService.js`:
```javascript
// For Android Emulator
const API_BASE_URL = "http://10.0.2.2/phpauthsystem/api";

// For Physical Device (replace with your IP)
const API_BASE_URL = "http://192.168.1.100/phpauthsystem/api";
```

**3. Start the development server**:
```bash
npx expo start
```

**4. Run on device**:
- Scan QR code with Expo Go app (iOS/Android)
- Press 'a' for Android emulator
- Press 'i' for iOS simulator

### Testing the App

**Test Scenarios**:

1. **Registration Flow**:
   - Try registering without filling all fields
   - Try mismatched passwords
   - Try password less than 6 characters
   - Successfully register a new user

2. **Login Flow**:
   - Try login with empty fields
   - Try wrong credentials
   - Successfully login with registered user

3. **Home Screen**:
   - Verify user email displays
   - Check all UI elements render
   - Test logout functionality

---

## Common Issues & Solutions

### API Connection Issues

**Problem**: "Network request failed"
**Solution**:
- Check API_BASE_URL is correct for your device type
- Ensure PHP server is running
- Verify firewall allows connections
- Test API with Postman first

### Styling Issues

**Problem**: Layout looks different on iOS vs Android
**Solution**:
- Use Platform-specific code when needed
- Test on both platforms
- Use elevation for Android, shadow for iOS

### State Not Updating

**Problem**: UI doesn't update when state changes
**Solution**:
- Ensure you're using setState function from useState
- Check that you're not mutating state directly
- Verify component is re-rendering

---

## Resources

### Official Documentation
- React Native: https://reactnative.dev/
- React Navigation: https://reactnavigation.org/
- Expo: https://docs.expo.dev/

### Learning Resources
- React Native Express (tutorial)
- Expo Snack (online playground)
- React Native School (courses)

### Tools
- React Native Debugger
- Expo DevTools
- VS Code with React Native extensions

---

## Quick Reference

### File Locations
- **App entry**: `App.js`
- **Screens**: `screens/` folder
- **API services**: `services/authService.js`
- **Configuration**: `package.json`

### Key Commands
```bash
npm install                    # Install dependencies
npx expo start                 # Start dev server
npx expo start --clear         # Clear cache and start
npm run android                # Run on Android
npm run ios                    # Run on iOS
```

### Important Props to Remember

**TextInput**:
- `value`, `onChangeText`, `placeholder`
- `secureTextEntry`, `keyboardType`, `autoCapitalize`

**TouchableOpacity**:
- `onPress`, `disabled`, `style`

**View**:
- `style` - Apply styles

**ScrollView**:
- `contentContainerStyle` - Style the content wrapper

---

## Summary

This app demonstrates:
- Complete authentication flow
- React Navigation implementation
- API integration with error handling
- Form validation and user feedback
- React Native styling patterns
- State management with hooks
- Platform-specific considerations

Students should understand how to:
- Build multi-screen apps
- Handle user input and validation
- Make API calls and handle responses
- Style components professionally
- Navigate between screens with data
- Provide good user experience

---

## Support

For questions about this app:
1. Check the console logs for debugging info
2. Review the code comments
3. Test API endpoints independently
4. Check React Native documentation

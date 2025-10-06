WeatherFest-Notifier 🌦️🎉

A robust Make.com automation that fetches real-time weather data for Hyderabad, India, and generates tailored marketing notifications based on weather conditions and local festivals. Perfect for Indian businesses looking to align promotions with weather and cultural events.

![Make.com](https://img.shields.io/badge/Make.com-Automation-6B46C1)
![AccuWeather](https://img.shields.io/badge/AccuWeather-API-FF6B35)
![Gemini AI](https://img.shields.io/badge/Gemini-AI-4285F4)


## 🎯 Features

- **Real-time Weather Integration**: Fetches current weather data (temperature, humidity, rain) from AccuWeather API
- **AI-Powered Festival Detection**: Uses Gemini AI (2.5 Flash and Pro) to identify significant festivals in Hyderabad
- **Dynamic Marketing Content**: Generates JSON-formatted marketing copy tailored to weather and festivals
- **Multi-channel Notifications**: Sends push notifications via Android and Firebase Cloud Messaging (FCM)
- **Router-based Architecture**: Flexible design for expanding to additional notification channels
- **Secure Credentials**: Handles API keys safely using Make.com variables

## 🚀 Use Cases

Perfect for:
- E-commerce teams promoting weather-appropriate products (e.g., waterproof gadgets during monsoons)
- Retail businesses running festival-specific campaigns
- Marketing teams targeting Hyderabad customers with contextual promotions
- Any Indian business wanting to align messaging with local weather and cultural events

## 📋 Prerequisites

Before setting up, ensure you have:

1. **Make.com Account** - [Sign up here](https://www.make.com)
2. **AccuWeather API Key** - [Get your key](https://developer.accuweather.com/)
3. **Google Cloud Account** - For Gemini AI and FCM
   - [Gemini AI API Access](https://ai.google.dev/)
   - [Firebase Cloud Messaging Setup](https://console.firebase.google.com/)
4. **Android Device** (optional) - For testing Android notifications

## ⚙️ Setup Instructions

### 1. Import the Blueprint

1. Download the blueprint JSON file from this repository
2. Log in to your Make.com account
3. Navigate to **Scenarios** → **Create a new scenario**
4. Click the three dots menu → **Import Blueprint**
5. Upload the downloaded JSON file

### 2. Configure API Connections

#### AccuWeather
1. In Make.com, go to **Connections** → **Add**
2. Create a new HTTP connection for AccuWeather
3. Define the variable `{{apiKey}}` with your AccuWeather API key

#### Gemini AI
1. Create a Gemini AI connection in Make.com
2. Authenticate using your Google Cloud credentials
3. Ensure the Gemini AI modules (IDs 8 and 9) use this connection

#### Firebase Cloud Messaging (FCM)
1. Create an FCM connection in Make.com
2. Define the following variables:
   - `{{Token}}`: Your FCM device registration token
   - `{{projectId}}`: Your Firebase project ID

#### Android (Optional)
1. Configure an Android device in Make.com
2. Follow Make.com's device setup instructions

### 3. Define Required Variables

In Make.com, navigate to **Variables** and create:

```json
{
  "apiKey": "your_accuweather_api_key",
  "Token": "your_fcm_registration_token",
  "projectId": "your_fcm_project_id",
  "locationCode": "202190"
}
```

| Variable | Description | Default | Required |
|----------|-------------|---------|----------|
| `apiKey` | AccuWeather API key | - | Yes |
| `Token` | FCM registration token | - | Yes |
| `projectId` | Firebase project ID | - | Yes |
| `locationCode` | AccuWeather location code | 202190 (Hyderabad) | No |

### 4. Update Location (Optional)

To use a different location:

1. Find your location code via [AccuWeather's Location API](https://developer.accuweather.com/accuweather-locations-api/apis)
2. Update `{{locationCode}}` in Make.com variables
3. Example codes:
   - Hyderabad: `202190`
   - Mumbai: `204842`
   - Delhi: `202396`

### 5. Test the Scenario

1. Click **Run once** in Make.com
2. Verify weather data is fetched successfully
3. Check that notifications are received on your device(s)
4. Review the generated marketing content

## 🔧 Configuration Tips

### Fix Variable Syntax in FCM Module

Update module ID 18 to use proper variable syntax:

```json
"mapper": {
  "registrationToken": "{{Token}}",
  "projectId": "{{projectId}}"
}
```

### Handle the OneSignal Module

The blueprint includes a disconnected OneSignal module. Choose one option:

**Option 1: Integrate for Email Notifications**

Add a route to the router (ID 15) and configure:

```json
"mapper": {
  "body": "{{13.tagline}}",
  "type": "email",
  "subject": "{{13.title}}",
  "included_segments": ["All Email Subscriptions"]
}
```

**Option 2: Remove**

Delete the orphans section from `metadata.designer`:

```json
"orphans": []
```

### Add Error Handling (Recommended)

Create a fallback route with a generic notification that triggers if weather or AI modules fail:

```json
{
  "mapper": {
    "projectId": "{{projectId}}",
    "notificationData": {
      "body": "Check out Getto's latest offers!",
      "title": "Weather Update Unavailable"
    },
    "registrationToken": "{{Token}}"
  }
}
```

Add a filter: Trigger only if `{{3.data}}` or `{{9.candidates}}` is empty.

## 📊 Scenario Flow

```
AccuWeather API → Weather Data → Gemini AI (Festival Check) → Gemini AI (Marketing Copy)
                                                                         ↓
                                                                    Router
                                                                   /      \
                                                          FCM Module    Android Module
```

## 🛠️ Module Notes

Add these notes in Make.com for better documentation:

- **Module 3**: Fetches real-time weather data for Hyderabad from AccuWeather
- **Module 8**: Uses Gemini AI to check if the date is a significant festival in Hyderabad
- **Module 9**: Generates marketing copy based on weather and festival data
- **Module 15**: Routes notifications to multiple channels
- **Module 16**: Sends FCM push notification
- **Module 17**: Sends Android notification

## ⚠️ Limitations

- Requires manual setup of API connections and variables
- JSON parsing assumes valid AI output (add error handling for production)
- Location is Hyderabad-specific by default (customizable via `{{locationCode}}`)
- OneSignal module is included but not active

## 🤝 Contributing

We welcome contributions! Here are some ways to improve the project:

- Add error handling for API failures
- Support additional locations with multi-location configurations
- Integrate other notification channels (email, SMS, WhatsApp)
- Add weather condition templates for different scenarios
- Improve festival detection logic

**To contribute:**

1. Fork this repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request


## 📧 Support

Having issues? Please open an issue on GitHub with:
- Make.com scenario screenshot
- Error messages or logs
- Steps to reproduce the problem

## 🙏 Acknowledgments

- [AccuWeather](https://www.accuweather.com/) for weather data
- [Google Gemini AI](https://ai.google.dev/) for intelligent content generation
- [Make.com](https://www.make.com/) for the automation platform
- [Firebase](https://firebase.google.com/) for push notification infrastructure

---

**Made with ❤️ for the Getto brand and Indian businesses**

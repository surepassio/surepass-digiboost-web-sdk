# Digiboost Web SDK - Complete Integration Guide

Welcome to the **Digiboost Web SDK**! This guide will walk you through a simple 3-step journey to integrate the Digiboost Web SDK into your web application.

## 🚀 Quick Overview

The Digiboost Web SDK enables secure document verification and digital identity services in your web application. Follow this guide to get up and running in minutes.

Please find the *[Documentation Link](https://console.surepass.app/product/console/api-lists?active=16301914&leafId=16301914&path=%2Fdocs%2Fkyc%2Finitialize-16301914e0&expanded=3588860%2C3588870)*.

Visit our Website *[Surepass.in](https://surepass.in)*.

---

## 📋 Prerequisites

- **Modern web browser** (Chrome 60+, Firefox 55+, Safari 12+, Edge 79+)
- **JavaScript enabled** in the browser
- **HTTPS environment** (required for production)
- **Web server** or local development environment

---

## 🎯 3-Step Integration Journey

### Step 1: Generating Your SDK Token

Before integrating the SDK, you need to obtain an authentication token from the Digiboost API.

#### 1.1 Get API Details from Your Sales Manager
Contact your sales manager to receive:
- Digilocker initialize endpoint URL
- **Authorization Bearer Token** (required for API access)
- Access permissions

#### 1.2 Environment Configuration

We provide two environments for different stages of development:

| Environment | Base URL | Usage |
|-------------|----------|-------|
| **UAT (Testing)** | `https://sandbox.surepass.app` | For development and testing |
| **Production** | `https://kyc-api.surepass.app` | For live applications |

#### 1.3 Digilocker Initialize API

**For UAT Environment:**
```bash
curl --location 'https://sandbox.surepass.app/api/v1/digilocker/initialize' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer TOKEN_GOT_FROM_SALES_MANAGER' \
--data '{
    "data": {
        "signup_flow": true,
        "logo_url": "YOUR BRAND LOGO URL",
        "skip_main_screen": false
    }
}'
```

**For Production Environment:**
```bash
curl --location 'https://kyc-api.surepass.app/api/v1/digilocker/initialize' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer TOKEN_GOT_FROM_SALES_MANAGER' \
--data '{
    "data": {
        "signup_flow": true,
        "logo_url": "YOUR BRAND LOGO URL",
        "skip_main_screen": false
    }
}'
```

#### 1.4 API Parameters Explanation

| Parameter | Type | Required | Description | Default Value |
|-----------|------|----------|-------------|---------------|
| `signup_flow` | boolean | ✅ **Required** | This parameter should always be `true` for SDK initialization | `true` |
| `logo_url` | string | ❌ Optional | Your branding logo URL - customize with your own logo |
| `skip_main_screen` | boolean | ❌ Optional | Whether to show the first intro screen or skip it | `true` |

#### 1.5 Customization Examples

**Basic Configuration (Minimal):**
```json
{
    "data": {
        "signup_flow": true
    }
}
```

**Custom Branding with Voice Assistant:**
```json
{
    "data": {
        "signup_flow": true,
        "logo_url": "https://yourcompany.com/logo.png",
        "skip_main_screen": false
    }
}
```

#### 1.6 API Response
You'll receive a response like this:

```json
{
  "data": {
    "client_id": "digilocker_cntWpMxWHbcvgghtyvxw",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "expiry_seconds": 600.0
  },
  "status_code": 200,
  "message_code": "success",
  "message": "Success",
  "success": true
}
```

**Important**:
- Copy the `token` value - you'll need this for Step 3!
- The token expires in 600 seconds (10 minutes) by default
- Store the `client_id` if needed for tracking purposes

---

### Step 2: Including the SDK

The Digiboost Web SDK is available via CDN for easy integration.

#### 2.1 CDN Integration (Recommended)

Add the SDK to your HTML page:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Digiboost Web SDK Demo</title>
</head>
<body>
    <!-- Your existing content -->
    
    <!-- Include Digiboost Web SDK -->
    <script src="https://your-cdn-url/digiboost-sdk.js"></script>
    
    <!-- Your application scripts -->
    <script src="app.js"></script>
</body>
</html>
```
---

## 🖼️ Sample UI Preview

Below is a sample of how the Digiboost Web SDK button  will look in your application:

![Digiboost Web SDK Sample UI](/assets/digilocker-flow.png)

---

### Step 3: SDK Integration & Implementation

Now let's integrate the SDK into your web application with proper initialization and response handling.

#### 3.1 Basic Implementation

```javascript
// Initialize the SDK with your token from Step 1
window.DigiboostSdk({
    gateway: "sandbox", // Use "production" for live environment
    token: "YOUR_TOKEN_FROM_STEP_1", // Replace with actual token
    selector: "#digilocker-button", // CSS selector where button should appear
    onSuccess: function(data) {
        console.log("Verification successful:", data);
        // Handle successful verification
        showSuccessMessage("Document verification completed successfully!");
    },
    onFailure: function(error) {
        console.log("Verification failed:", error);
        // Handle verification failure
        showErrorMessage("Verification was cancelled or failed. Please try again.");
    }
});
```

#### 3.2 Advanced Implementation with Custom Styling

```javascript
window.DigiboostSdk({
    gateway: "sandbox", // Use "production" for live environment
    token: "YOUR_TOKEN_FROM_STEP_1", // Replace with actual token
    selector: ".auth-container", // CSS selector for mounting
    style: {
        backgroundColor: "#4CAF50",
        color: "white",
        padding: "15px 30px",
        borderRadius: "8px",
        fontSize: "16px",
        fontWeight: "bold",
        boxShadow: "0 4px 8px rgba(0,0,0,0.2)",
        transition: "all 0.3s ease"
    },
    onSuccess: function(data) {
        // Handle successful verification
        console.log("User verified:", data);
        
        // Example: Update UI
        document.getElementById("status").innerHTML = "✅ Verified";
        
        // Example: Redirect user
        window.location.href = "/dashboard";
    },
    onFailure: function(error) {
        // Handle verification failure
        console.log("Verification cancelled or failed:", error);
        
        // Example: Show error message
        alert("Verification was cancelled. Please try again.");
    }
});
```

#### 3.3 Complete HTML Example

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Digiboost Web SDK Demo</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 800px;
            margin: 0 auto;
            padding: 20px;
        }
        .container {
            text-align: center;
            margin-top: 50px;
        }
        #status {
            margin: 20px 0;
            padding: 10px;
            border-radius: 5px;
        }
        .success { background-color: #d4edda; color: #155724; }
        .error { background-color: #f8d7da; color: #721c24; }
    </style>
</head>
<body>
    <div class="container">
        <h1>Digiboost Web SDK Demo</h1>
        <p>Click the button below to start document verification:</p>
        
        <div id="digilocker-button"></div>
        
        <div id="status"></div>
    </div>

    <!-- Include Digiboost Web SDK -->
    <script src="https://your-cdn-url/digiboost-sdk.js"></script>
    
    <script>
        // Initialize SDK
        window.DigiboostSdk({
            gateway: "sandbox",
            token: "YOUR_TOKEN_FROM_STEP_1", // Replace with actual token
            selector: "#digilocker-button",
            style: {
                backgroundColor: "#2196F3",
                color: "white",
                padding: "12px 24px",
                borderRadius: "25px",
                fontSize: "14px",
                fontWeight: "600",
                boxShadow: "0 2px 4px rgba(33, 150, 243, 0.3)",
                transition: "all 0.2s ease"
            },
            onSuccess: function(data) {
                console.log("Verification successful:", data);
                document.getElementById("status").innerHTML = 
                    '<div class="success">✅ Verification completed successfully!</div>';
            },
            onFailure: function(error) {
                console.log("Verification failed:", error);
                document.getElementById("status").innerHTML = 
                    '<div class="error">❌ Verification failed or was cancelled</div>';
            }
        });
    </script>
</body>
</html>
```

---

## 📋 Configuration Options

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `gateway` | string | ✅ | Environment specification - use `"sandbox"` for testing or `"production"` for live environment |
| `token` | string | ✅ | Authentication token for the verification process |
| `selector` | string | ❌ | CSS selector where the button should be mounted (defaults to `body`) |
| `style` | object | ❌ | Custom CSS styles to apply to the button |
| `onSuccess` | function | ❌ | Callback function called on successful verification |
| `onFailure` | function | ❌ | Callback function called on verification failure or cancellation |

---

## 🎨 Customizing SDK Appearance

### Default Styles
```javascript
{
    padding: "10px 20px",
    backgroundColor: "#613AF5",
    color: "white",
    border: "none",
    borderRadius: "6px",
    cursor: "pointer",
    width: "100%"
}
```

### Custom Styling Examples

**Modern Blue Theme:**
```javascript
style: {
    backgroundColor: "#2196F3",
    color: "white",
    padding: "12px 24px",
    borderRadius: "25px",
    fontSize: "14px",
    fontWeight: "600",
    boxShadow: "0 2px 4px rgba(33, 150, 243, 0.3)",
    transition: "all 0.2s ease"
}
```

**Green Success Theme:**
```javascript
style: {
    backgroundColor: "#4CAF50",
    color: "white",
    padding: "15px 30px",
    borderRadius: "8px",
    fontSize: "16px",
    fontWeight: "bold",
    boxShadow: "0 4px 8px rgba(0,0,0,0.2)",
    transition: "all 0.3s ease"
}
```

---

## 🔄 Event Handling

### Success Callback
```javascript
onSuccess: function(data) {
    // data contains the verification result
    console.log("Verification data:", data);
    
    // Example: Update UI
    document.getElementById("status").innerHTML = "✅ Verified";
    
    // Example: Redirect user
    window.location.href = "/dashboard";
    
    // Example: Send to server
    fetch("/api/verify", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(data)
    });
}
```

### Failure Callback
```javascript
onFailure: function(error) {
    // Called when user cancels or verification fails
    console.log("Verification failed:", error);
    
    // Example: Show error message
    alert("Verification was cancelled. Please try again.");
    
    // Example: Update UI
    document.getElementById("status").innerHTML = "❌ Verification failed";
}
```

---

## 🛡️ Security Features

- **Origin Validation**: Messages are only accepted from the authorized DigiLocker domain
- **Popup Management**: Automatic cleanup of popup windows
- **Event Cleanup**: Proper removal of event listeners to prevent memory leaks
- **HTTPS Required**: Production environment requires HTTPS for security

---

## 🌐 Browser Compatibility

- ✅ Chrome 60+
- ✅ Firefox 55+
- ✅ Safari 12+
- ✅ Edge 79+

---

## 🔧 Troubleshooting

### Common Issues and Solutions

#### Issue: "Token expired" error
**Solution**:
- Generate a new token using Step 1
- Tokens expire after the specified time (usually 10 minutes)

#### Issue: SDK not launching
**Solution**:
- Verify the token format is correct
- Check that the SDK script is properly loaded
- Ensure you're using HTTPS in production

#### Issue: Popup blocked
**Solution**:
- Allow popups for your domain in browser settings
- Ensure the user interaction (click) triggers the popup

#### Issue: Button not appearing
**Solution**:
- Check that the selector exists in your HTML
- Verify the SDK script is loaded before initialization
- Check browser console for JavaScript errors

#### Issue: "Origin validation failed"
**Solution**:
- Ensure you're using the correct gateway URL
- Check that your domain is whitelisted (if required)

---

## 📱 Testing Your Integration

1. **Open your web page** in a modern browser
2. **Click the "Verify with DigiLocker" button**
3. **Verify the popup opens** successfully
4. **Complete a test verification** to ensure the full flow works
5. **Check the browser console** for response data

---

## 📞 Support

If you encounter any issues:
- **Check the troubleshooting section** above
- **Contact your sales manager** for API-related issues
- **Contact Tech Support at** <techsupport@surepass.app>
- **Review the browser console** for detailed error messages

---

## 📄 License

This SDK is provided as-is for integration purposes.

---

**✨ You're all set! Your Digiboost Web SDK integration is complete.**

The SDK will automatically:
- Create a verification button
- Handle popup management
- Process verification responses
- Clean up resources after completion

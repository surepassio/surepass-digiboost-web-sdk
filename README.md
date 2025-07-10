# DigiLocker JS SDK (CDN Integration)

Embed a secure DigiLocker KYC button in your HTML or React app via CDN. This SDK opens a popup flow and returns success or failure through JavaScript callbacks.

---

## ✅ Features

- Plug-and-play `<script>` tag integration
- Automatically renders a styled button
- Supports success/failure callbacks
- Allows mounting to specific elements
- Customizable text and styles via `DigiLockerSDK` global API

---

## 🚀 Quick Start (HTML)

```html
<script src="https://cdn.jsdelivr.net/gh/surepassio/surepass-digiboost-web-sdk@latest/index.min.js"></script>
<script>
	function handleSuccess(result) {
		console.log(result, "Result");
	}
	function handleFailure(error) {
		console.log(error, "DError");
    }
	DigiboostSdk({
			gateway: "GATEWAY",//sandbox or production
			token: "Token",//initialize token from digilocker init
			onSuccess: handleSuccess,
			onFailure: handleFailure,
			selector: "CSS Selector" //CSS selector where the SDK will inject the DigiLocker button
    });
</script>

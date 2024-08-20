To create a custom function in Google Sheets that calls the OpenAI API with content from a cell and retrieves a response, you can use Google Apps Script. 

Here's a step-by-step guide to setting this up:

### 1. Set Up OpenAI API Key
Before you begin, ensure you have an API key from OpenAI. If you don't have one, you'll need to sign up on OpenAI's platform and generate an API key.

### 2. Open Google Sheets and Go to the Script Editor
1. Open your Google Sheets document.
2. Click on `Extensions` in the menu.
3. Select `Apps Script`. This opens the Google Apps Script editor.

### 3. Write the Custom Function
In the Apps Script editor, you will write a script that calls the OpenAI API. Here's a basic example:

```javascript
// Define your OpenAI API key
const OPENAI_API_KEY = 'YOUR_OPENAI_API_KEY';

function callOpenAI(prompt) {
  // Define the API endpoint
  const url = 'https://api.openai.com/v1/chat/completions';

  // Set up the headers and the payload for the API call
  const headers = {
    "Authorization": `Bearer ${OPENAI_API_KEY}`,
    "Content-Type": "application/json"
  };

  const payload = {
    model: "gpt-4o", // Use your desired model
    messages: [{"role": "user", "content": prompt}],
    max_tokens: 100 // Adjust as necessary
  };

  // Make the API request
  const options = {
    method: 'POST',
    headers: headers,
    payload: JSON.stringify(payload)
  };

  try {
    const response = UrlFetchApp.fetch(url, options);
    const json = JSON.parse(response.getContentText());

    // Extract the response from the API (assuming it's in the 'choices' array)
    return json.choices[0].message.content.trim();
  } catch (error) {
    return `Error: ${error}`;
  }
}

function ChatGPT(cellContent) {
  return callOpenAI(cellContent);
}
```

### 4. Save the Script
1. Name your script (e.g., `OpenAI API Integration`).
2. Save your work by clicking the floppy disk icon or using `Ctrl + S`.

### 5. Use the Custom Function in Google Sheets
Now you can use the custom function `=GPT(A1)` in your Google Sheets. Replace `A1` with the reference to the cell containing the prompt you want to send to the OpenAI API.

### 6. Handling Limitations and Considerations
- **API Usage**: Be mindful of your API usage and limits. Google Sheets might make multiple calls depending on how you use the function.
- **Error Handling**: The script includes basic error handling, but you may want to expand this based on your needs.
- **Response Length**: Adjust `max_tokens` in the payload to control the length of the response from OpenAI.

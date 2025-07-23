---
date: '2025-07-22T23:33:41-05:00'
draft: false
title: 'AI-Powered Incident Description Enhancement with Claude AI'
cover:
    image: "/gifs/ClaudeAI_EnchanceDescription_HD.gif"
    alt: "A short video demonstrating a ServiceNow UI Action titled 'Enhance Description with Claude AI'. The video shows a user clicking the UI Action on an Incident record, which then presents an AI-generated, improved version of the incident description. The user then accepts the suggestion, updating the description field."
    caption: "A quick look at the 'Enhance Description with Claude AI' UI Action in ServiceNow."
    hidden: false
    hiddenInSingle: false
summary: "Integrates Claude AI into ServiceNow via a custom UI Action and REST API to enhance incident descriptions, improving clarity, consistency, and technician efficiency."
ShowReadingTime: false
tags: ["ServiceNow", "UI Action", "Claude AI", "Incident Management", "AI Integration"]
---
### The Objective

ITIL technicians often spend valuable time drafting or refining incident descriptions for clarity and completeness, which can lead to delayed resolution and inconsistent data quality. My goal was to explore leveraging AI to quickly enhance incident descriptions, improving efficiency and data consistency from the initial reporting stage.

### The Solution & Technical Details

I developed a UI Action, "Enhance Description with Claude AI," on the Incident table in my PDI. This solution integrates ServiceNow with Anthropic's Claude AI via a REST API, allowing technicians to generate improved incident descriptions with a single click.

Key functionalities include:

* **UI Action:** Trigger on the Incident form.
* **Secure REST API Integration:** Script Include handles outbound REST API calls to Claude AI for text processing and securely manages API keys.
* **Automated Description Update:** The enhanced text from Claude automatically updates the Incident's "Description" field for technician review.

### Code Snippet Showcase

This snippet from the Script Include demonstrates the core logic for making the REST API call to Claude AI:

```javascript
var ClaudeUtils = Class.create();
ClaudeUtils.prototype = {
    initialize: function() {
    },

    enhanceIncidentDescription: function(originalText) { // Only takes originalText
        var enhancedText = "Error: Could not enhance description with Claude.";
        // Simple, hardcoded prompt prefix for now
        var promptPrefix = "Rewrite the following IT incident description to be more professional, detailed, and clear. Focus on extracting key information and presenting it suitable for an IT support team. Original description: ";
        var fullPrompt = promptPrefix + originalText;

        // gs.debug("ClaudeUtils - Reverted Full Prompt: " + fullPrompt); // Optional debug

        try {
            // Ensure these names EXACTLY match your REST Message and HTTP Method configuration
            var r = new sn_ws.RESTMessageV2('Claude', 'POST');

            // The variable name 'incident_text_to_enhance' must match the
            // ${variable_name} in your REST Message's HTTP Method "Content" field.
            r.setStringParameterNoEscape('incident_text_to_enhance', fullPrompt);

            var response = r.execute();
            var responseBody = response.getBody();
            var httpStatus = response.getStatusCode();

            // gs.debug("ClaudeUtils - Reverted API Status: " + httpStatus + ", Response: " + responseBody); // Optional debug

            if (httpStatus == 200) {
                var parsedBody = JSON.parse(responseBody);
                if (parsedBody && parsedBody.content && parsedBody.content[0] && parsedBody.content[0].text) {
                    enhancedText = parsedBody.content[0].text;
                    enhancedText = enhancedText.trim();
                } else {
                    gs.error("ClaudeUtils (Reverted): Could not parse enhanced text from Claude response. Body: " + responseBody);
                    enhancedText = "Error: Claude AI response format unexpected (reverted).";
                }
            } else {
                gs.error("ClaudeUtils (Reverted): Claude API request failed. Status: " + httpStatus + ", Body: " + responseBody);
                enhancedText = "Error: Claude AI API request failed with status " + httpStatus + " (reverted). Details: " + responseBody;
            }

        } catch (ex) {
            gs.error("ClaudeUtils (Reverted): Exception during Claude API call: " + ex.getMessage());
            enhancedText = "Error: Exception during Claude AI call (reverted) - " + ex.getMessage();
        }
        return enhancedText;
    },

    type: 'ClaudeUtils'
};
```

### Future Enchancements
I would refactor the integration to use Integration Hub for more robust, low-code management, ensure the API key is securely stored in a system property, and pull relevant caller information from the ticket to provide Claude AI with additional context for even more detailed and accurate description enhancements.
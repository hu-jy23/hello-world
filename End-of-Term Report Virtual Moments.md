## End-of-Term Report: Virtual Moments

### Project Title: Virtual Moments
### Team Members: 
- 谢旻洋
- 胡加怿
- 赵梦洁

### Project Overview
Virtual Moments is an AI Conversational Agent designed to create a character-based interactive social media environment. The project simulates a virtual social network where users can interact with characters from various media through chatting, viewing posts, liking, and commenting.

### Project Components and Detailed Implementation

#### 1. **ChatUI.html**
**Purpose:** 
- This HTML file constructs the main structure of the user interface for the Virtual Moments application.

**Key Elements:**
- **Header Section:** Displays the user's profile picture and navigation icons.
- **Chat List:** A dynamically generated list of characters that the user can interact with. Each list item includes the character's name, location, and profile picture.
- **Chat Interface:** A chat window where conversations with selected characters are displayed.
- **Chat Input:** An input box for the user to type messages and a send button to submit them.

**Detailed Structure:**
- **Header Section:** Contains a user's profile image and navigation icons linking to other parts of the application.
- **Chat List:** 
  - Contains a series of `<li>` elements, each representing a different character.
  - Each character block includes an image, name, and location.
- **Chat Interface:**
  - Displays the selected character's profile picture and name with an online status indicator.
  - Contains a chat box where messages are displayed.
  - Includes an input field and send button for user messages.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Virtual Moments</title>
    <link rel="stylesheet" href="css/chatstyles.css">
</head>
<body>
<main>
    <div class="container">
        <div class="chatleft">
            <div class="header">
                <div class="img">
                <img src="../webpage/photos/user.png" class="cover">
                </div>
                <div class="icons">
                    <a href="../webpage/VM.html">
                    <li><img src="icons/vm_white.png"></li>
                    </a>
                </div>
            </div>
            <div class="chatlist">
                <!-- character list can be generated dynamically -->
                <li id="Furina" class="block">
                    <div class="imgbx">
                        <img src="../webpage/photos/furina.png" class="cover">
                    </div>  
                    <h4>Furina</h4>  
                    <div class="location">Fountaine</div>
                </li>
                <!-- Additional character list items -->
            </div>
        </div>
    </div>
    </div>
</main>
    <script src="js/chatscript.js"></script>
</body>
</html>
```

#### 2. **script.js**
**Purpose:** 
- This JavaScript file provides the functionality for the web application, including initializing the feed, handling user interactions, and communicating with the backend server.

**Key Functions:**
- **Initialization:** Automatically creates initial posts for specific characters when the page loads.
- **Fetch Response:** Sends a prompt to the server and retrieves a response.
- **Like Button Functionality:** Handles the like button's state and fetches comments in response to likes.
- **Comment Handling:** Manages user comments, including posting comments to the server and updating the UI.
- **Create New Post:** Dynamically generates new posts based on predefined character and friend pairs.
- **Event Listeners:** Handles various user interactions like clicking the follow button, liking posts, and submitting comments.

**Detailed Implementation:**
- **Initialization Function:**
  - Creates predefined posts for characters when the page loads.
- **Fetch Response Function:**
  - Sends a GET request to the server with the user's prompt and character information.
  - Retrieves and returns the server's response.
- **Like Button Functionality:**
  - Toggles the like button's state and updates the UI.
  - Fetches a comment from the server and displays it in response to a like.
- **Comment Handling:**
  - Captures user comments and sends them to the server.
  - Updates the UI with the new comments.
- **Create New Post:**
  - Dynamically creates new posts with user-generated content.
  - Adds new posts to the feed and sets up event listeners for interaction.

```javascript
// Initialize the feed
document.addEventListener('DOMContentLoaded', () => {
    createNewPost('Furina', 'photos/fpost.png', 'Fountaine', 'How do you like Fountaine\'s Star -- Furina\'s new outfit~', {'Neuvillete': "Furina's new dress is fantastic!"});
    // Other new posts
});

// Fetch response from the server
async function fetchResponse(prompt, character = 'None'){
    const encodedPrompt = encodeURIComponent(prompt);
    const url = `http://localhost:54226/answer?prompt=${encodedPrompt}&character=${character}`;

    try{
        const response = await fetch(url, {
            method: 'GET',
            headers: {
                'Content-Type': 'application/json',
            },
        });
        const data = await response.json();
        return data.message;
    }catch(error){
        console.error('Error:', error);
        return null;
    }
}

// Toggle follow button
document.querySelectorAll('.suggestion button').forEach(button => {
    // Utilization function
});

// Like button functionality
async function handleLikeButtonClick(button) {
    // Generate a comment response from post sender
}

document.querySelectorAll('.like-button').forEach(button => {
    button.addEventListener('click', async function () {
        if (this.classList.contains('liked')) {
            this.classList.remove('liked');
            this.innerText = 'Like';
        } else {
            this.classList.add('liked');
            this.innerText = 'Liked';

            // Handle the like button click
            await handleLikeButtonClick(this);
        }
    });
});

// Comment functionality
document.querySelectorAll('.comment-input').forEach(input => {
    // Add user's comment and give feedback
});

// Function to update comment visibility
function updateCommentVisibility(commentsContainer) {
   	// Utilization function
}

// Create a new post
function createNewPost(username, photoPath, location, caption, comments = {});

const cfPairs = [
    { character: 'Furina', friend: 'Neuvillete', place: 'Fountaine'},
    { character: 'Tighnari', friend: 'Corei', place: 'Sumeru'},
    { character: 'Iron', friend: 'Pepper', place: 'The U.S.'},
    { character: 'Jack', friend: 'Sailor', place: 'The Atlantic Ocean' },
    { character: 'Jimmy', friend: 'Rachel', place: 'Tokyo' },
    { character: 'Nick', friend: 'Judy', place: 'Zootopia' },
]
```

#### 3. **webapi.py**
**Purpose:** 
- This Python script serves as the backend for the Virtual Moments application, handling the processing of user inputs and generating responses based on the Phi-3 model.

**Key Functions:**
- **Request Handling:** Manages incoming GET and POST requests from the front end.
- **Response Generation:** Uses the Phi-3 model to generate appropriate responses to user prompts.
- **Integration with Front End:** Communicates with the JavaScript front end to provide seamless interaction.

**Detailed Implementation:**

- **Endpoint Definitions:** Defines endpoints for handling different types of requests.
- **Model Interaction:** Interfaces with the Phi-3 model to generate responses based on user inputs and context.
- **Error Handling:** Includes mechanisms to handle and log errors during request processing.

```python
def chat_resp(model, tokenizer, user_prompt=None, history=[]):
    # The response generator
    pipe = pipeline(
        "text-generation",
        model=model,
        tokenizer=tokenizer,
    )   
    generation_args = {
        "max_new_tokens": 100,
        "return_full_text": False,
        "temperature": 0.6,
        "do_sample": True,
    }
    messages = history
    if user_prompt:
        prompt_msg = [{"role": "user", "content": user_prompt}]
        messages.extend(prompt_msg)
    output = pipe(messages, **generation_args)
    return output

# One example of context learning function
@app.post("/chat/")
async def chat(data: dict):
    prompt = data.get('prompt', '')
    character = data.get('character', 'Furina').lower()
    history = [{"role": "system", "content": f"Do a role play and play as character {character}. Learn from the following QA examples, then chat with the user in a similar tone and each answer shoule be within 50 words:"},]
    file_path = os.path.join(current_dir, '../webpage/text', f'{character}.txt')
    with open(file_path, 'r', encoding='utf-8') as file:
        lines = [line.strip() for line in file.readlines()]

    for i in range(0, len(lines), 2):
        # This is context leanring part
        history.append({"role": "user", "content": lines[i]})
        history.append({"role": "assistant", "content": lines[i+1]})
    
    history.append({"role": "system", "content": "Now, chat with the user in a similar tone as the QA examples above and each answer should be within 50 words:"})
        
    histories = data.get('history', [])
    history+=histories
    print(history)
    resp = chat_resp(model, tokenizer, prompt, history)
    return resp[0]['generated_text'].replace('"', '')
```



### Project Achievements and Learnings

#### Achieved Features
1. **Personalized Virtual Friends:** Successfully created a system to add and interact with friends based on popular characters.
2. **Varied Virtual Social Networking Capabilities:** Implemented features for chatting, viewing posts, liking, commenting, and receiving replies.
3. **Immersive and Authentic Virtual Social Environment:** Developed a realistic social media interface with smooth operations and detailed character information.

#### Technical Learnings
1. **Prompt Engineering:** Utilized prompt engineering for effective interaction with AI models.
2. **Memory Management:** Implemented memory management techniques to handle user interactions efficiently.
3. **Stable Diffusion Pipeline:** Applied stable diffusion techniques for generating images, though with noted instability in outputs.
4. **Web API Service:** Integrated HTML for front-end development, complementing custom HTML and CSS coding; Web API  completes the backend text generation and can work together.
5. **Fine-Tuning Challenges:** Encountered difficulties with fine-tuning models, leading to a preference for context learning over fine-tuning.

#### Positive and Negative Experiences
1. **Positive:** Context learning with the Phi-3 model performed better than fine-tuning, demonstrating the potential for fewer-shot learning in specific contexts.
2. **Negative:** Fine-tuning models were resource-intensive and sometimes resulted in poor performance. HTML and CSS development proved laborious but necessary for a polished user interface.

### Conclusion
The Virtual Moments project successfully created an engaging and immersive virtual social environment powered by AI.

 The team overcame challenges in model fine-tuning and front-end development to deliver a compelling user experience. This project highlights the potential of combining AI with web technologies to create dynamic and interactive social platforms. The learnings from this project will inform future developments and improvements in virtual social interactions.
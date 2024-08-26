# mapletesting
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Comment Section</title>
    <style>
        html, body {
            margin: 0;
            padding: 0;
            height: 100%;
            box-sizing: border-box;
        }
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f4;
            display: flex;
            flex-direction: column;
            min-height: 100vh;
            margin: 0; /* Remove default body margin */
        }
        .container {
            width: 100%;
            max-width: 1200px;
            margin: auto;
            padding: 20px;
            background: white;
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
            border-radius: 8px;
            display: flex;
            flex-direction: column;
            flex: 1;
        }
        h1 {
            text-align: center;
            color: #333;
            margin-top: 0;
        }
        .comments-section {
            flex: 1;
            overflow-y: auto;
            margin-bottom: 20px;
        }
        .comment {
            padding: 10px;
            border-bottom: 1px solid #ddd;
            margin-bottom: 10px;
            border-radius: 4px;
            background: #fafafa;
        }
        .comment p {
            margin: 0;
        }
        .comment .author {
            font-weight: bold;
            color: #555;
        }
        .comment .text {
            margin-top: 5px;
        }
        .comment img {
            max-width: 100%;
            height: auto;
            margin-top: 10px;
            border-radius: 4px;
        }
        .form-group {
            margin-bottom: 15px;
        }
        .form-group label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
        }
        .form-group input, .form-group textarea {
            width: 100%;
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 4px;
        }
        .form-group textarea {
            height: 80px; /* Adjust height for better fit */
            resize: vertical;
        }
        .form-group button {
            background-color: #5cb85c;
            color: white;
            border: none;
            padding: 10px 20px;
            font-size: 16px;
            cursor: pointer;
            border-radius: 4px;
            margin-top: 10px;
        }
        .form-group button:hover {
            background-color: #4cae4c;
        }
        .form-group input[type="file"] {
            padding: 0;
        }
        .form-container {
            margin-top: 20px; /* Space above the form */
        }
        @media (max-width: 768px) {
            .form-group textarea {
                height: 60px; /* Adjust textarea height for smaller screens */
            }
        }
    </style>
</head>
<body>

    <div class="container">
        <h1>Comment Section</h1>
        <div class="comments-section" id="comments-section">
            <!-- Comments will appear here -->
        </div>
        <div class="form-container">
            <div class="form-group">
                <label for="name">Name (Optional):</label>
                <input type="text" id="name" placeholder="Enter your name">
            </div>
            <div class="form-group">
                <label for="comment">Comment:</label>
                <textarea id="comment" placeholder="Enter your comment"></textarea>
            </div>
            <div class="form-group">
                <label for="image">Upload Image (Optional):</label>
                <input type="file" id="image" accept="image/*">
            </div>
            <div class="form-group">
                <button onclick="addComment()">Submit Comment</button>
            </div>
        </div>
    </div>

    <script>
        function loadComments() {
            const comments = JSON.parse(localStorage.getItem('comments')) || [];
            const commentsSection = document.getElementById('comments-section');
            commentsSection.innerHTML = ''; 

            comments.forEach(comment => {
                const commentElement = document.createElement('div');
                commentElement.className = 'comment';
                commentElement.innerHTML = `
                    <p class="author">${comment.name} says:</p>
                    <p class="text">${comment.text}</p>
                    ${comment.image ? `<img src="${comment.image}" alt="Uploaded Image">` : ''}
                `;
                commentsSection.appendChild(commentElement);
            });
        }

        function addComment() {
            const name = document.getElementById('name').value.trim();
            const commentText = document.getElementById('comment').value.trim();
            const imageInput = document.getElementById('image');
            const displayName = name === '' ? 'Anonymous' : name;

            if (commentText === '') {
                alert('Please enter a comment.');
                return;
            }

            const newComment = {
                name: displayName,
                text: commentText
            };

            if (imageInput.files.length > 0) {
                const file = imageInput.files[0];
                const reader = new FileReader();
                
                reader.onloadend = function () {
                    newComment.image = reader.result; 
                    saveComment(newComment);
                };

                reader.readAsDataURL(file);
            } else {
                saveComment(newComment);
            }
        }

        function saveComment(comment) {
            const comments = JSON.parse(localStorage.getItem('comments')) || [];
            comments.push(comment);

            localStorage.setItem('comments', JSON.stringify(comments));

            document.getElementById('name').value = '';
            document.getElementById('comment').value = '';
            document.getElementById('image').value = ''; 

            loadComments();
        }

        window.onload = loadComments;
    </script>

</body>
</html>

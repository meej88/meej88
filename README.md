<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>GPT4 Chat API Model</title>
    <link rel="stylesheet" href="./style.css">
    <link rel="icon" href="./favicon.ico" type="image/x-icon">
  </head>
  <body>
    <main>
        <h1>ChatGPT4 API</h1>  
		<form action="">
			<input type="text" name="message" id="message">
			<button type="submit">Send</button>
		</form>
		<div id="chat-log">

		</div>
    </main>
	<script>
		let messages = []
		const chatLog = document.getElementById('chat-log');
		const message = document.getElementById('message');
		const form = document.querySelector('form');
		form.addEventListener('submit', (e) => {
			e.preventDefault();
			const messageText = message.value;
			const newMessage = {"role": "user", "content": `${messageText}`}
			messages.push(newMessage)
			message.value = '';
			const messageElement = document.createElement('div');
			messageElement.classList.add('message');
			messageElement.classList.add('message--sent');
			messageElement.innerHTML = `
				<div class="message__text">${messageText}</div>
			`;
			chatLog.appendChild(messageElement);
			chatLog.scrollTop = chatLog.scrollHeight;
			fetch('https://adrianazuregpt.azurewebsites.net/api/gptfunction?', {
				method: 'POST',
				headers: {
					'Content-Type': 'application/json'
				},
				body: JSON.stringify({
					messages
				})
			})
			.then(res => res.json())
			.then(data => {
				let newAssistantMessage = {"role": "assistant", "content": `${data.completion.content}`}
				messages.push(newAssistantMessage)
				const messageElement = document.createElement('div');
				messageElement.classList.add('message');
				messageElement.classList.add('message--received');
				messageElement.innerHTML = `
					<div class="message__text">${data.completion.content}</div>
				`;
				chatLog.appendChild(messageElement);
				chatLog.scrollTop = chatLog.scrollHeight;
			})
		})
	</script>
  </body>
</html>

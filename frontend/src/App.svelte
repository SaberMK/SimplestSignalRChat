<script>
	import * as signalR from '@aspnet/signalr'
	let message = ''
	
	var connection = new signalR.HubConnectionBuilder()
    													.configureLogging(signalR.LogLevel.Debug)
															.withUrl('http://localhost:52348/messages', {
																skipNegotiation: true,
																transport: signalR.HttpTransportType.WebSockets
															})
															.build();

	var messages = [];
	connection.on("ReceiveMessage", (message) => {
		messages = [...messages, message]
	})


	connection.start().catch((err) => console.error(err.toString()))

	const send = () => {
		connection.invoke("SendMessageToAll", message)
		console.log('send', message);
		message = '';
	}
</script>

<main>
	<textarea name="message" id="message" bind:value={message}></textarea>
	<input type="button" id="sendButton" value="Send message" on:click={send}/>
	<br/>

	{#each messages as message}
	<p>{message}</p>
	{/each}
</main>



<style>
</style>
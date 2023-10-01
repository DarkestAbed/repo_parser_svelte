<script>
    let url;

    async function sendData(data) {
        if (url === "" || url === null || url === undefined) {
            alert("El campo está vacío");
        } else {
            try {
                const repoPath = getURL(url);
                const repoReplaced = repoPath.replace("/", "&sol");
                const urlPOST = `http://127.0.0.1:8000/put-new-repo/${repoReplaced}`;
                // Process to fend fetch
                let request = new Request(urlPOST, {
                    method: "POST",
                    body: JSON.stringify(data),
                    headers: new Headers({
                        "Content-Type": "application/json; charset=latin-1",
                        'Access-Control-Allow-Origin': '*'
                    })
                });
                // execution
                console.log("Sending data");
                const response = await fetch(request);
                console.log(response);
                if (!response.ok) {
                    alert(`El repo ${url} no pudo ser agregado`);
                } else {
                    alert(`Yay! El repo ${url} fue agregado con éxito`);
                }
            } catch (error) {
                console.log(error);
            }
        };
    };

    function getURL(url) {
        console.log(url);
        try {
            const newURL = new URL(url);
            if (newURL.hostname != "github.com" && newURL.hostname != "www.github.com") {
                alert("La URL entregada no es de GitHub");
                throw new Error("Not a valid GitHub URL");
            };
            if (newURL.pathname.substring(1) === "") {
                alert("La URL entregada no corresponde a un repo de GitHub");
                throw new Error("Not a valid GitHub repo URL");
            };
            if (!newURL.pathname.substring(1).includes("/")) {
                alert("La URL entregada no corresponde a un repo de GitHub");
                throw new Error("Not a valid GitHub repo URL");
            };
            return newURL.pathname.substring(1);
        } catch (error) {
            alert(`La URL ${url} no es una dirección válida`)
            throw new Error(error);
        };
    };

    function reset() {
        url = "";
    }

    function buttonClic() {
        try {
            sendData({test: "test"});
            reset();
        } catch (error) {
            console.log(error)
        }
    }
</script>

<h2>Ingresa un nuevo repo aquí</h2>
<form id="add-repo">
    <label for="url-input">URL:</label><br>
    <input type="text" id="url-input" name="url-input" style="width: 400px;" bind:value={url}>
    <button type="button" id="add-btn" on:click={buttonClic}>Agregar repo</button>
</form>

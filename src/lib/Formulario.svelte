<script>
    let items = [];
    let titulo = "";
    let descripcion = "";
    let tecnologia = "";
    let bandera = false;

    const cleanForm = () => {
        titulo = "";
        descripcion = "";
        tecnologia = "";
    };
    const data = async () => {
        try {
            const response = await fetch(
                "https://render-books.onrender.com/api/projects/"
            );
            const books = await response.json();
            console.log(books);
            items = books;
        } catch (error) {
            console.log(error);
        }
    };
    const dataTitulo = (event) => {
        titulo = event.target.value;
        console.log(titulo);
    };
    const dataDesripcion = (event) => {
        descripcion = event.target.value;
        console.log(descripcion);
    };
    const dataTecnologia = (event) => {
        tecnologia = event.target.value;
        console.log(tecnologia);
    };
    data();

    $: if (
        titulo.length === 0 ||
        descripcion.length === 0 ||
        tecnologia.length === 0
    ) {
        bandera = false;
    } else {
        bandera = true;
        console.log({
            title: titulo,
            Desc: descripcion,
            tec: tecnologia,
        });
    }

    const submit = () => {
        const dataBooks = {
            titulo: titulo,
            descripcion: descripcion,
            tecnologia: tecnologia,
        };
        const options = {
            method: "POST",
            headers: {
                Accept: "application/json",
                "Content-Type": "application/json",
            },
            body: JSON.stringify({
                titulo,
                descripcion,
                tecnologia,
            }),
        };
        fetch("https://render-books.onrender.com/api/projects/", options)
            .then((response) => response.json())
            .then(() => {
                data();
                cleanForm();
            })
            .catch((err) => console.error(err));
    };
</script>

<div class="container espacio">
    <h2 class="mb-5 mt-5">Ejercicio API/REST SVELTE-DJANGORESTFRAMEWORK</h2>
    <div class="row">
        <div class="col">
            <form on:submit|preventDefault={submit}>
                <div class="card tarjeta">
                    <div class="card-body tarjeta">
                        <h5 class="card-title text-uppercase text-center">
                            Libros - sebashvare
                        </h5>
                        <hr />
                        <h6 class="text-muted card-subtitle mb-2">
                            Formulario
                        </h6>
                        <p class="card-text small">
                            Registraremos a manera de ejercicio los libros desde
                            el frontend creado en Svelte, comunicandose en el
                            Backend con DjangoRestFramework 🚀
                        </p>
                        <div class="input-group mb-3">
                            <span class="input-group-text">📕</span>
                            <input
                                class="form-control form-control-sm"
                                type="text"
                                placeholder="Nombre del libro .."
                                bind:value={titulo}
                            />
                        </div>
                        <div class="input-group mb-3">
                            <span class="input-group-text">📕</span>
                            <input
                                class="form-control form-control-sm"
                                type="text"
                                placeholder="Descripcion del libro.."
                                bind:value={descripcion}
                            />
                        </div>
                        <select class="mb-3" bind:value={tecnologia}>
                            <optgroup label="Seleccionar Grupo Libro">
                                <option value="Tecnologia">Tecnologia</option>
                                <option value="Finanzas">Finanzas</option>
                                <option value="Politica">Politica</option>
                            </optgroup>
                        </select>
                        <button
                            disabled={!bandera}
                            class="btn btn-primary"
                            type="submit">guardar libro</button
                        >
                    </div>
                </div>
            </form>
        </div>
    </div>
    <hr />
    <div class="row mt-5">
        {#if items.length > 0}
            <div class="col">
                <div class="card tarjeta">
                    <div class="card-body tarjeta">
                        <h5 class="card-title text-uppercase text-center">
                            Resultados Libros
                        </h5>
                        <hr />
                        {#each items as item}
                            <p>
                                <span class="badge text-bg-danger"
                                    >{item.id}</span
                                > <br />
                                {item.titulo} <br />
                                {item.descripcion} <br />
                                {item.tecnologia} <br />
                            </p>
                            <hr />
                        {/each}
                    </div>
                </div>
            </div>
        {:else}
            <p>No hay libros por el momento</p>
        {/if}
    </div>
</div>

<style>
    p {
        text-align: center;
    }
    .card .tarjeta p {
        padding: 10px;
        text-align: center;
    }
    .tarjeta {
        max-width: 600px;
        width: 100%;
    }
</style>

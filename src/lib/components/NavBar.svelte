<script>
    import { page } from "$app/stores";
    import logo from "$lib/assets/supadrop-logo.png"
</script>

<nav>
    <div class="logo">
        <a href="/" class="logo">
            <!-- <img src={logo} alt="supadrop-logo"> -->
            <h3 class="h3 font-bold font-sans">SupaDrop</h3>
        </a>
    </div>
    <div class="nav-links">
        <a href="/upload">
            <span class={$page.url.pathname === '/upload' ? "badge preset-filled" : "badge preset-outlined"}>
                <p class="text-sm">upload</p>
            </span>
        </a>
        <a href="/download">
            <span class={$page.url.pathname === '/download' ? "badge preset-filled" : "badge preset-outlined"}>
                <p class="text-sm">download</p>
            </span>
        </a>
    </div>
</nav>

<style>
    nav {
        display: flex;
        align-items: center;
        justify-content: space-between;
        padding: 1rem;
    }

    .logo {
        display: flex;
        align-items: center;
        gap: 1rem;
    }

    .nav-links {
        display: flex;
        gap: 1rem;
    }
</style>
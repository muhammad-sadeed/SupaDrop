<script>
    import { page } from "$app/stores";
    import logo from "$lib/assets/supadrop-logo.png"
</script>

<nav>
    <div class="logo">
        <a href="/" class="logo">
            <img src={logo} alt="supadrop-logo">
            <h2>SupaDrop</h2>
        </a>
    </div>
    <div class="nav-links">
        <li>
            <a href="/upload" class:active={$page.url.pathname === '/upload'}>Upload</a>
        </li>
        <li>
            <a href="/download" class:active={$page.url.pathname === '/download'}>Download</a>
        </li>
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

    .logo img {
        height: 2.5rem;
    }

    .nav-links {
        display: flex;
        gap: 1rem;
    }

    .nav-links li, a {
        all: unset;
        text-decoration: none;
    }

    li, a:hover {
        cursor: pointer;
    }

    nav h2 {
        margin: 0;
    }

    a.active {
		text-decoration: underline;
	}
</style>
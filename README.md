<picture>
<img src="https://raw.githubusercontent.com/MoonKraken/DrawsNotes/main/demo.gif" />
</picture>

# DrawsNotes

More details about the creation of this project can be found in [this video](https://youtu.be/Pr6T0Phjvgc).

A [later video](https://youtu.be/zLTaN5fJUY8?si=VC4r-pVeFnuMIfnC) demonstrated how to deploy it with Terraform and GitLab CI/CD.

A very simple note-taking app built with an all-Rust stack: Dioxus for the frontend and backend (Axum under the hood on the backend), and SurrealDB as the database.

1. `cargo install dioxus-cli` *Run this again even if you've done it before, outdated versions may cause breakage*
1. `rustup target add wasm32-unknown-unknown`
1. Install SurrealDB (on a mac, `brew install surrealdb/tap/surreal`)
1. Start surrealdb with no authentication `surreal start --unauthenticated` *We assume it is running on the same machine as DrawsNotes, you'll need to make a code change if it is to run elsewhere*
1. In this project directory
    1. `npx tailwindcss -i ./input.css -o ./public/tailwind.css --watch`
    1. `dx serve`
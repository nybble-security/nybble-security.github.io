# README.md
# Nybble — Jekyll Starter (Landing + Blog)


## Prérequis
- Ruby + Bundler installés


## Dev local (avec auto-reload)
```
bundle install
bundle exec jekyll serve --livereload
```


Ouvrez http://127.0.0.1:4000


## Déploiement GitHub Pages
- Poussez ce repo sur GitHub.
- Paramètres → Pages → « Deploy from a branch » → `main` / `/ (root)`.
- CNAME : `nybble-security.io` si domaine custom.


## Contenu
- **Landing** : `index.html`
- **Blog** : posts dans `_posts/` (`YYYY-MM-DD-titre.md`)
- **Liste blog** : `blog/index.html`


## SEO
- `{% seo %}` est inclus (jekyll-seo-tag) + `sitemap.xml` auto.


## Formulaire
- Remplacez l’URL Formspree dans la section `#contact`.
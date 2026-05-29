# CVnikki

CV personal — Nichita Ciobu. Site static deployat pe DigitalOcean (nginx).

## Structura
- `cv-nichita-ciobu.html` — pagina CV-ului
- nginx pe droplet serveste fisierul ca index

## Update flow
1. Editezi `cv-nichita-ciobu.html` local
2. `git add -A && git commit -m "..." && git push`
3. Pe droplet: `cd /var/www/CVnikki && git pull` (sau hook automat)

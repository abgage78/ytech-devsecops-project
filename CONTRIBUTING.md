# Guide de contribution

Merci de votre intérêt pour contribuer au projet Ytech Solutions !

## Comment contribuer

### Signaler un bug

1. Vérifiez que le bug n'a pas déjà été signalé dans les Issues
2. Créez une nouvelle Issue avec le label `bug`
3. Décrivez le bug en détail :
   - Étapes pour reproduire
   - Comportement attendu
   - Comportement actuel
   - Environnement (OS, version k3s, etc.)

### Proposer une fonctionnalité

1. Créez une Issue avec le label `enhancement`
2. Décrivez la fonctionnalité proposée
3. Expliquez le cas d'usage
4. Attendez les retours de la communauté

### Soumettre une Pull Request

1. Fork le projet
2. Créez une branche depuis `main` :
   ```bash
   git checkout -b feature/ma-fonctionnalite
   ```
3. Faites vos modifications
4. Testez vos changements
5. Commit avec un message clair :
   ```bash
   git commit -m "feat: ajout support monitoring Prometheus"
   ```
6. Push vers votre fork :
   ```bash
   git push origin feature/ma-fonctionnalite
   ```
7. Ouvrez une Pull Request vers `main`

### Convention de nommage des commits

Utilisez [Conventional Commits](https://www.conventionalcommits.org/) :

- `feat:` Nouvelle fonctionnalité
- `fix:` Correction de bug
- `docs:` Documentation
- `style:` Formatage
- `refactor:` Refactoring
- `test:` Tests
- `chore:` Maintenance

### Code de conduite

- Soyez respectueux et professionnel
- Acceptez les critiques constructives
- Focalisez sur ce qui est le mieux pour le projet
- Montrez de l'empathie envers les autres contributeurs

## Questions ?

N'hésitez pas à ouvrir une Issue avec le label `question`.

Merci de contribuer ! 🙏

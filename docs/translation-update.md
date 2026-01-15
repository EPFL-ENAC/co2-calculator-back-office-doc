## How to update texts and translation in the app
To update translation, first goes to the link in which the text is located through backoffice Documentation-editing translation section.
## 1. Edit text button 

<img width="890" height="388" alt="Image" src="https://github.com/user-attachments/assets/cee74a22-27e2-44e7-9af0-17dfabcfe94e" />

## 2. Make your changes
Be careful with:

-  `\n` pour un retour à la ligne dans un paragraphe. 
<img width="1349" height="83" alt="Image" src="https://github.com/user-attachments/assets/111f14c1-dbd0-46b9-b980-1ecbec686be4" />


- {count} ne jamais changer éléments entre curly brackets (permet d'afficher des informations dynamiquement)
<img width="233" height="67" alt="Image" src="https://github.com/user-attachments/assets/7151286f-6e8f-4baf-b4f2-1ea3d18ff496" />

- `’` pour les apostrophes en français (l’exemple) et non` '` (l'exemple) qui ferme les strings.
- Si besoin d'utiliser `'` ( let's) le texte doit se trouver dans des `"`  Exemple : `" Let's go "` et non `'Let's go'`
<img width="521" height="250" alt="Image" src="https://github.com/user-attachments/assets/25f61af2-b80a-40c3-9356-7e37d7b0ea52" />

## 3. Commit changes

<img width="882" height="417" alt="Image" src="https://github.com/user-attachments/assets/48a95980-5f63-4f5b-81bf-6e2c46dfe541" />


## 4. Make a commit message
Courte description ds changements apportés (où, quoi), ajouter une description si besoin.

<img width="488" height="651" alt="Image" src="https://github.com/user-attachments/assets/138e872a-d354-41e0-93bc-013184895098" />

## 5. Create new branch
Créer une nouvelle branch avec la syntax `text/<edited_files>`

<img width="509" height="674" alt="Image" src="https://github.com/user-attachments/assets/225158a4-5ef3-4e99-8224-f3e0ae5573fc" />

## 6. When to do the PR
Vous arriverez ensuite sur `Open a Pull Request`
- Mettez un titre, vous pouvez ajouter une description ou laisser vide, ou laisser le template.
- Mettez @charlottegiseleweil  comme reviewer en haut à droite
- Créer la Pull Request en bas à droite
<img width="1295" height="827" alt="Image" src="https://github.com/user-attachments/assets/59d8edb3-ddbd-4682-84ed-cb793d1b9ef0" />

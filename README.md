# TodoApp

This project was generated using [Angular CLI](https://github.com/angular/angular-cli) version 20.0.5.

## 🧱 1. Architecture du projet

```
todo-app/
├── src/
│   └── app/
│       ├── core/               # Services globaux (ex : auth.service.ts)
│       ├── auth/               # Composants standalone : login/logout
│       ├── tasks/              # Composants standalone : liste & formulaire des tâches
│       ├── shared/             # Pipes/directives réutilisables
│       ├── app.config.ts       # Configuration Angular (routes, providers)
│       ├── app.routes.ts       # Routes principales
│       ├── app.ts              # Composant racine (standalone)
│       └── main.ts             # Point d’entrée de l’app (bootstrap)
```

---

## 🚀 2. Création du projet

```bash
ng new todo-app --standalone --routing=false --style=css
cd todo-app
ng serve
```

---

## 📄 Étapes

```
🔐 AuthService (estion connexion / déconnexion)
📥 LoginComponent (formulaire de connexion)
✅ LogoutComponent
📥 Register (formulaire d inscription)
🧠 TaskService (gestion des tâches)
📝 TaskListComponent (affichage + ajout/suppression de tâches)
🧹 CapitalizePipe (pipe personnalisé)
```

## 🧰 3. Génération des composants & services

### 🔐 Auth (login/logout)

```bash
ng generate component auth/login --standalone
ng generate component auth/logout --standalone
ng generate component auth/register --standalone
ng generate service core/auth
```

### ✅ Tasks (tâches)

```bash
ng generate component tasks/task-list --standalone
ng generate component tasks/task-form --standalone
ng generate service tasks/task
```

### 📦 Partagé

```bash
ng generate pipe shared/capitalize --standalone
```

---

## 🧭 4. Routing (Standalone)

### 📄 `src/app/app.routes.ts`

```ts
import { Routes } from "@angular/router";
import { Login } from "./auth/login/login";
import { Logout } from "./auth/logout/logout";
import { TaskList } from "./tasks/task-list/task-list";
import { Register } from "./auth/register/register";

export const routes: Routes = [
  { path: "", redirectTo: "tasks", pathMatch: "full" },
  { path: "register", component: Register },
  { path: "login", component: Login },
  { path: "logout", component: Logout },
  { path: "tasks", component: TaskList },
  { path: "**", redirectTo: "tasks" },
];
```

## ⚙️ 5. Configuration de l'application

### 📄 `src/app/app.config.ts`

````ts
import { ApplicationConfig } from '@angular/core';
import { provideRouter } from '@angular/router';
import { routes } from './app.routes';

export const appConfig: ApplicationConfig = {
  providers: [provideRouter(routes)]
};

---

## 🌱 6. inscription

- Composant `Register` standalone avec formulaire + styles
- Méthode simple `register()` dans `src/app/core/auth.ts`
- Route `/register`
- Lien dans la navbar

### 📄 `src/app/auth/register.ts`

```ts
import { Component } from "@angular/core";
import { Router } from "@angular/router";
import { CommonModule } from "@angular/common";
import { FormsModule } from "@angular/forms";
import { Auth } from "../../core/auth";

@Component({
  selector: "app-register",
  standalone: true,
  imports: [CommonModule, FormsModule],
  templateUrl: "./register.html",
  styleUrl: "./register.css",
})
export class Register {
  username = "";
  email = "";
  password = "";

  constructor(private auth: Auth, private router: Router) {}

  register() {
    const success = this.auth.register(this.username, this.email, this.password);
    if (success) {
      alert("Inscription réussie ! Vous pouvez maintenant vous connecter.");
      this.router.navigate(["/login"]);
    } else {
      alert("Erreur lors de l'inscription.");
    }
  }
}
````

---

# 3. Ajouter méthode `register` dans `auth.ts`

Dans `src/app/core/auth.ts` ajoute :

```ts
register(username: string, email: string, password: string): boolean {
  console.log('Inscription:', username, email, password);
  return true;
}
```

---

# 4. Ajouter la route dans `auth-routes.ts`

```ts
import { Routes } from "@angular/router";
import { LoginComponent } from "./login";
import { RegisterComponent } from "./register";

export const authRoutes: Routes = [
  { path: "login", component: LoginComponent },
  { path: "register", component: RegisterComponent }, // nouvelle route
  { path: "logout", component: LogoutComponent },
];
```

---

# 5. Ajouter lien vers l’inscription dans la navbar ou login

Exemple dans navbar (`navbar.ts`) :

```html
<a routerLink="/login" *ngIf="!auth.isAuthenticated()">Login</a>
<a routerLink="/register" *ngIf="!auth.isAuthenticated()">Inscription</a>
<a (click)="logout()" *ngIf="auth.isAuthenticated()">Logout</a>
```

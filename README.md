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
ng new todo-angular --standalone --routing=false --style=css
cd todo-angular
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
import { Routes } from '@angular/router';
import { LoginComponent } from './auth/login/login.component';
import { LogoutComponent } from './auth/logout/logout.component';
import { TaskListComponent } from './tasks/task-list/task-list.component';

export const routes: Routes = [
  { path: '', redirectTo: 'tasks', pathMatch: 'full' },
  { path: 'login', component: LoginComponent },
  { path: 'logout', component: LogoutComponent },
  { path: 'tasks', component: TaskListComponent },
  { path: '**', redirectTo: 'tasks' }
];
```

---

## ⚙️ 5. Configuration de l'application

### 📄 `src/app/app.config.ts`

```ts
import { ApplicationConfig } from '@angular/core';
import { provideRouter } from '@angular/router';
import { routes } from './app.routes';

export const appConfig: ApplicationConfig = {
  providers: [provideRouter(routes)]
};
```

---

## 🧩 6. Composant racine

### 📄 `src/app/app.ts`

```ts
import { Component } from '@angular/core';
import { RouterOutlet } from '@angular/router';
import { Navbar } from '../app/navbar';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [RouterOutlet, Navbar],
  templateUrl: './app.html',
  styleUrl: './app.css'
})
export class App {
  protected title = 'todo-app';
}

```

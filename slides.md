---
colorSchema: light
---

# Anatomia de um Component

Component é um building block fundamental do Angular. Composto de 3 partes:
- Typescript class
- HTML template
- CSS styles

````md magic-move
```ts
@Component({})
export class AppComponent {}
```

```ts
import {Component} from '@angular/core';

@Component({
  selector: '',
  template: ``, // HTML template
  styles: ``, // CSS styles
  standalone: true
})
export class AppComponent {} // TS Class
```
````

---

## Alterando HTML template

E CSS style.

> Podemos usar todo HTML e CSS suportado pelo browsers.

````md magic-move
```ts
import {Component} from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
    Hello Universe
  `,
  styles: ``,
  standalone: true
})
export class AppComponent {}
```

```ts
import {Component} from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
    Hello Universe
  `
  styles: `
    :host {
      color: #a144eb;
    }
  `
  standalone: true
})
export class AppComponent {}
```
````

---

# Atualizando o Component Class

- `city` é uma propriedade da classe `AppComponent` (**class property**)
- Podemos utilizar o valor de `city` (**class property**) num **string interpolation**
- **Interpolation** aceita elementos do TS, como `{{ 1 + 1 }}`

````md magic-move
```ts
import {Component} from '@angular/core';

@Component({})
export class AppComponent {
  city = 'Xique-Xique'
}
```

```ts
import {Component} from '@angular/core';

@Component({
  template: `Hello {{ city }}`
})
export class AppComponent {
  city = 'Xique-Xique'
}
```

```ts
import {Component} from '@angular/core';

@Component({
  template: `Hello {{ city }}, {{ 1 + 1 }}`
})
export class AppComponent {
  city = 'Xique-Xique'
}
```

```ts
import {Component} from '@angular/core';

@Component({
  selector: 'app-root',
  template: `Hello {{ city }}, {{ 1 + 1 }}`,
  styles: ``,
  standalone: true
})
export class AppComponent {
  city = 'Xique-Xique'
}
```
````

---

# Composing Components

Como podemos utilizar um componente em outro componente?

1. Declare o component que vai ser reutilizado (`UserComponent`)
2. Declare o component que vai utilizá-lo (`AppComponent`)
3. `AppComponent` vai importar `UserComponent` (**uses reference**)
4. `AppComponent` vai utilizar o **selector** `app-user` definido em `UserComponent`
5. Podemos usar markups HTML e duplicar quantas vezes o uso do `<app-user />`

````md magic-move
```ts
@Component({
  selector: 'app-user'
})
export class UserComponent {}
```

```ts
@Component({
  selector: 'app-user'
})
export class UserComponent {}

@Component({
  selector: 'app-root'
})
export class AppComponent {}
```

```ts
@Component({
  selector: 'app-user'
})
export class UserComponent {}

@Component({
  selector: 'app-root'
  imports: [UserComponent]
})
export class AppComponent {}
```

```ts
@Component({
  selector: 'app-user',
  template: `Username: John`
})
export class UserComponent {}

@Component({
  selector: 'app-root',
  template: `<app-user />`,
  imports: [UserComponent]
})
export class AppComponent {}
```

```ts
@Component({
  selector: 'app-root',
  template: `<section><app-user /></section>`,
  imports: [UserComponent]
})
export class AppComponent {}
```

```ts
@Component({
  selector: 'app-root',
  template: `
    <section>
      <app-user />
      <app-user />
    </section>
  `,
  imports: [UserComponent]
})
export class AppComponent {}
```
````

---

# Control Flow em Components 

## `@if`

- `@` : prefixo para **template syntax** do Angular
- `@if` e `@else`: são chamados **template syntax**
- versões antes de v16, era usado o **Ngif**

````md magic-move
```ts
@Component({
  template: `
    <span>Welcome back, Friend!</span>
  `,
})
export class AppComponent {}
```

```ts
@Component({
  template: `
    <span>Welcome back, Friend!</span>
  `,
})
export class AppComponent {
  isLoggedIn = true;
}
```

```ts
@Component({
  template: `
    @if (isLoggedIn) {
      <span>Welcome back, Friend!</span>
    }
  `,
})
export class AppComponent {
  isLoggedIn = true;
}
```

```ts
@Component({
  template: `
    @if (isLoggedIn) { <span>Welcome back, Friend!</span> }
    @else { <p>Please, sign in</p> }
  `,
})
export class AppComponent {
  isLoggedIn = false;
}
```
````

---

## `@for`

````md magic-move
```ts
@Component({
  template: ``
})
export class AppComponent {}
```

```ts
@Component({
  template: ``
})
export class AppComponent {
  operatingSystems = [
    {
      id: 'win',
      name: 'Windows'
    },
    {
      id: 'osx',
      name: 'MacOS'
    },
    {
      id: 'linux',
      name: 'Linux'
    }
  ]
}
```

```ts
@Component({
  template: `
    @for (os of operatingSystems;) {}
  `
})
export class AppComponent {
  operatingSystems = [
    {
      id: 'win',
      name: 'Windows'
    },
    {
      id: 'osx',
      name: 'MacOS'
    },
    {
      id: 'linux',
      name: 'Linux'
    }
  ]
}
```

```ts
@Component({
  template: `
    @for (os of operatingSystems; track os.id) {}
  `
})
export class AppComponent {
  operatingSystems = [
    {
      id: 'win',
      name: 'Windows'
    },
    {
      id: 'osx',
      name: 'MacOS'
    },
    {
      id: 'linux',
      name: 'Linux'
    }
  ]
}
```

```ts
@Component({
  template: `
    @for (os of operatingSystems; track os.id) {
      {{ os.name }}
    }
  `
})
export class AppComponent {
  operatingSystems = [
    {
      id: 'win',
      name: 'Windows'
    },
    {
      id: 'osx',
      name: 'MacOS'
    },
    {
      id: 'linux',
      name: 'Linux'
    }
  ]
}
```
````

---

# Property Binding (Vinculação de Propriedade) `[binding]=`

Property binding permite setar valores para **propriedades** e **atributos** dinamicamente em:
- Elementos HTML
- Angular Components
- ...

````md magic-move
```ts
@Component({
  template: `<div contentEditable="true"></div>`
})
export class AppComponent {}
```

```ts
@Component({
  template: `<div contentEditable="true"></div>`
})
export class AppComponent {
  isEditable = true;
}
```

```ts
@Component({
  template: `<div [contentEditable]="isEditable"></div>`
})
export class AppComponent {
  isEditable = true;
}
```
````

---

# Event Handling

````md magic-move
```ts
@Component({})
export class AppComponent {}
```

```ts
@Component({
  template: `
    <section>{{ message }}</section>
  `
})
export class AppComponent {
  message = ''
}
```

```ts
@Component({
  template: `
    <section (mouseover)="onMouseOver()">{{ message }}</section>
  `
})
export class AppComponent {
  message = ''
}
```

```ts
@Component({
  template: `
    <section (mouseover)="onMouseOver()">{{ message }}</section>
  `
})
export class AppComponent {
  message = ''

  onMouseOver() {
    this.message = "Mouse over triggered"
  }
}
```
````

---

# Component Communication com `@Input`

- `@Input` define uma propriedade para fazer com o Component aceite:
  - Receber dados de alguma fonte
  - Receber dados de um Parent Component. Isso permite comunicação entre Parent e Child Component.

````md magic-move
```ts
// File `app/app.component.ts`
@Component({})
export class AppComponent {}

// File `app/user.component.ts`
@Component({})
export class UserComponent {}
```

```ts
// File `app/app.component.ts`
import { UserComponent } from './user.component';

@Component({
  imports: [UserComponent]
})
export class AppComponent {}

// File `app/user.component.ts`
@Component({})
export class UserComponent {}
```

```ts
// File `app/app.component.ts`
import { UserComponent } from './user.component';

@Component({
  template: `
    <app-user />
  `,
  imports: [UserComponent]
})
export class AppComponent {}

// File `app/user.component.ts`
@Component({})
export class UserComponent {}
```

```ts
// File `app/app.component.ts`
import { UserComponent } from './user.component';

@Component({
  template: `
    <app-user occupation="Angular Developer"/>
  `,
  imports: [UserComponent]
})
export class AppComponent {}

// File `app/user.component.ts`
@Component({})
export class UserComponent {
  @Input() occupation = '';
}
```

```ts
// File `app/app.component.ts`
import { UserComponent } from './user.component';

@Component({
  template: `
    <app-user occupation="Angular Developer"/>
  `,
  imports: [UserComponent]
})
export class AppComponent {}

// File `app/user.component.ts`
@Component({
  template: `<p>The user's occupation is {{ occupation }}.`
})
export class UserComponent {
  @Input() occupation = '';
}
```
````

---

# Communicating com `@Output`

- O decorator `@Output` permite que um Component notifique um outro caso um evento aconteça
- Uma class property decorada com `@Output` **gera eventos** que podem ser escutados pelo Parent Component
- `.emit()` provoca trigger

---

````md magic-move 
```ts
// File `app/app.component.ts`
@Component({})
export class AppComponent {}

// File `app/child.component.ts`
@Component({})
export class ChildComponent {}
```

```ts
// File `app/app.component.ts`
@Component({
  template: `
    <app-child />
    <p>🐢 all the way down {{ items.length }}</p>
  `
})
export class AppComponent {}

// File `app/child.component.ts`
@Component({})
export class ChildComponent {}
```

```ts
// File `app/app.component.ts`
@Component({
  template: `
    <app-child />
    <p>🐢 all the way down {{ items.length }}</p>
  `
})
export class AppComponent {
  items = new Array();
}

// File `app/child.component.ts`
@Component({})
export class ChildComponent {}
```

```ts
// File `app/app.component.ts`
import { ChildComponent } from './child.component';

@Component({
  template: `
    <app-child />
    <p>🐢 all the way down {{ items.length }}</p>
  `,
  imports: [ChildComponent]
})
export class AppComponent {
  items = new Array();
}

// File `app/child.component.ts`
@Component({
  selector: 'app-child'
})
export class ChildComponent {}
```

```ts
// File `app/app.component.ts`
import { ChildComponent } from './child.component';

@Component({
  template: `
    <app-child (addItemEvent)="addItem($event)" />
    <p>🐢 all the way down {{ items.length }}</p>
  `,
  imports: [ChildComponent]
})
export class AppComponent {
  items = new Array();

  addItem(item: string) {
    this.items.push(item);
  }
}

// File `app/child.component.ts`
@Component({
  selector: 'app-child'
})
export class ChildComponent {}
```

```ts
// File `app/app.component.ts`
import { ChildComponent } from './child.component';

@Component({
  template: `
    <app-child (addItemEvent)="addItem($event)" />
    <p>🐢 all the way down {{ items.length }}</p>
  `,
  imports: [ChildComponent]
})
export class AppComponent {
  items = new Array();

  addItem(item: string) {
    this.items.push(item);
  }
}

// File `app/child.component.ts`
@Component({
  selector: 'app-child',
  template: `<button (click)="addItem()">Add Item</button>`
})
export class ChildComponent {}
```

```ts
// File `app/app.component.ts`
// ...

// File `app/child.component.ts`
@Component({
  selector: 'app-child',
  template: `<button (click)="addItem()">Add Item</button>`
})
export class ChildComponent {
  addItem() {}
}
```

```ts
// File `app/app.component.ts`
// ...

// File `app/child.component.ts`
@Component({
  selector: 'app-child',
  template: `<button (click)="addItem()">Add Item</button>`
})
export class ChildComponent {
  addItem() {
    this.addItemEvent.emit('🐢');
  }
}
```

```ts
// File `app/app.component.ts`
// ...

// File `app/child.component.ts`
@Component({
  selector: 'app-child',
  template: `<button (click)="addItem()">Add Item</button>`
})
export class ChildComponent {
  @Output addItemEvent = new EventEmitter<string>();

  addItem() {
    this.addItemEvent.emit('🐢');
  }
}
```
````

---

# Deferrable Views (Visualizações adiáveis)

- Gostaríamos de adiar carregamentos de Components por alguns motivos (otimização)
- Block `@defer`: permite adiar o carregamento de um Component. Só vai carregar quando o **browser** estiver em **idle**
- Block `@placeholder`: adicionado ao bloco `@defer`, permite fazer eager load
- Block `@loading`: adicionar html enquanto **deferred component** está carregando
- Block `@defer (on viewport)`: quando **viewport** exibir o **deferred component** (ou seja, estar visível na tela), dispare o **loading**

> Para ver `@defer (on viewport)` em ação, precisa ter um HTML que provoque aumento do tamanho vertical da página HTML e assim
permitir scrolling.

---

````md magic-move
```ts
// Child component: File app/comments.component.ts
@Component({})
export class CommentsComponent {}

// Parent component: File app/app.component.ts
@Component({})
export class AppComponent {}
```

```ts
// Child component: File app/comments.component.ts
@Component({})
export class CommentsComponent {}

// Parent component: File app/app.component.ts
import { CommentsComponent } from './comments.component'

@Component({})
export class AppComponent {}
```

```ts
// Child component: File app/comments.component.ts
@Component({
  selector: 'comments'
})
export class CommentsComponent {}

// Parent component: File app/app.component.ts
import { CommentsComponent } from './comments.component'

@Component({
  template: `
    <comments/>
  `
  imports: [CommentsComponent]
})
export class AppComponent {}
```

```ts
// Child component: File app/comments.component.ts
@Component({
  selector: 'comments'
})
export class CommentsComponent {}
```

```ts
// Child component: File app/comments.component.ts
@Component({
  selector: 'comments',
  template: `
    <ul>
      <li>Building for the web is fantastic!</li>
      <li>The new template syntax is great</li>
      <li>I agree with the other comments!</li>
    </ul>
  `,
})
export class CommentsComponent {}
```

```ts
// Child component: File app/comments.component.ts
// ...

// Parent component: File app/app.component.ts
import { CommentsComponent } from './comments.component'

@Component({
  template: `
    <comments/>
  `
  imports: [CommentsComponent]
})
export class AppComponent {}
```

```ts
// Child component: File app/comments.component.ts
// ...

// Parent component: File app/app.component.ts
import { CommentsComponent } from './comments.component'

@Component({
  template: `
    @defer {
      <comments/>
    }
  `
  imports: [CommentsComponent]
})
export class AppComponent {}
```

```ts
// Child component: File app/comments.component.ts
// ...

// Parent component: File app/app.component.ts
import { CommentsComponent } from './comments.component'

@Component({
  template: `
    @defer {
      <comments/>
    }
  `
  imports: [CommentsComponent]
})
export class AppComponent {}
```

```ts
// Child component: File app/comments.component.ts
// ...

// Parent component: File app/app.component.ts
import { CommentsComponent } from './comments.component'

@Component({
  template: `
    @defer {
      <comments/>
    } @placeholder {
      <p>Future comments</p> 
    }
  `
  imports: [CommentsComponent]
})
export class AppComponent {}
```

```ts
// Child component: File app/comments.component.ts
// ...

// Parent component: File app/app.component.ts
import { CommentsComponent } from './comments.component'

@Component({
  template: `
    @defer {
      <comments/>
    } @placeholder {
      <p>Future comments</p> 
    } @loading {
      <p>Loading comments...<p> 
    }
  `
  imports: [CommentsComponent]
})
export class AppComponent {}
```

```ts
// Child component: File app/comments.component.ts
// ...

// Parent component: File app/app.component.ts
import { CommentsComponent } from './comments.component'

@Component({
  template: `
    @defer {
      <comments/>
    } @placeholder {
      <p>Future comments</p> 
    } @loading (minimum 2s) {
      <p>Loading comments...<p> 
    }
  `
  imports: [CommentsComponent]
})
export class AppComponent {}
```

```ts
@Component({
  template: `
    <div>
      <h1>How I feel about Angular</h1>
      <article>
        <p>
          Angular is my favorite framework, and this is why. Angular has the coolest deferrable view
          feature that makes defer loading content the easiest and most ergonomic it could possibly
          be. The Angular community is also filled with amazing contributors and experts that create
          excellent content. The community is welcoming and friendly, and it really is the best
          community out there.
        </p>
        <p>Angular is my favorite framework...</p>
        ...
      </article>
      @defer {
        <comments/>
      } @placeholder {
        <p>Future comments</p> 
      } @loading (minimum 2s) {
        <p>Loading comments...<p> 
      }
    </div>
  `
})
export class AppComponent {}
```

```ts
@Component({
  template: `
    <div>
      <h1>How I feel about Angular</h1>
      <article>
        <p>Angular is my favorite framework...</p>
        ...
      </article>
      @defer {
        <comments/>
      } @placeholder {
        <p>Future comments</p> 
      } @loading (minimum 2s) {
        <p>Loading comments...<p> 
      }
    </div>
  `
})
export class AppComponent {}
```

```ts
@Component({
  template: `
    <div>
      <h1>How I feel about Angular</h1>
      <article>
        <p>Angular is my favorite framework...</p>
        ...
      </article>
      @defer (on viewport) {
        <comments/>
      } @placeholder {
        <p>Future comments</p> 
      } @loading (minimum 2s) {
        <p>Loading comments...<p> 
      }
    </div>
  `
})
export class AppComponent {}
```
````

---

# Optimizing Images

Lidar com imagens é algo complexo na Web. Imagens são muito utilizadas e fonte de problemas de otimização.
Performance ruim provoca redução do score no [Core Web Vitals](https://web.dev/explore/learn-core-web-vitals).

- A diretiva `NgOptimizedImage`: garante carregamento de imagem mais eficiente
- Lembre-se:
  - `src`: static image source
  - `[src]`: dynamic image source
- `NgOptimizedImage`: vai nos obrigar a colocar atributos `width` e `height` para evitar o problema do [layout shift - CLS](https://web.dev/articles/cls) (Mudança de layout)
- Não quer colocar `width` e `height`?
  - Use atributo `fill`

---

````md magic-move
```ts
@Component({
  template: `
    <img src="/assets/logo.svg" alt="Angular logo" />
    <img [src]="logoUrl" [alt]="logoAlt" />
  `
})
export class UserComponent {}
```

```ts
@Component({
  template: `
    <img src="/assets/logo.svg" alt="Angular logo" />
    <img [src]="logoUrl" [alt]="logoAlt" />
  `
})
export class UserComponent {
  logoUrl = '/assets/logo.svg';
  logoAlt = 'Angular logo';
}
```

```ts
@Component({
  template: `
    <img ngSrc="/assets/logo.svg" alt="Angular logo" />
    <img [ngSrc]="logoUrl" [alt]="logoAlt" />
  `
})
export class UserComponent {
  logoUrl = '/assets/logo.svg';
  logoAlt = 'Angular logo';
}
```

```ts
import { NgOptimizedImage } from '@angular/common';

@Component({
  template: `
    <img ngSrc="/assets/logo.svg" alt="Angular logo" />
    <img [ngSrc]="logoUrl" [alt]="logoAlt" />
  `,
  imports: [NgOptimizedImage]
})
export class UserComponent {
  logoUrl = '/assets/logo.svg';
  logoAlt = 'Angular logo';
}
```

```ts
import { NgOptimizedImage } from '@angular/common';

@Component({
  template: `
    <img ngSrc="/assets/logo.svg" alt="Angular logo" width="32" height="32" />
    <img [ngSrc]="logoUrl" [alt]="logoAlt" width="32" height="32" />
  `,
  imports: [NgOptimizedImage]
})
export class UserComponent {
  logoUrl = '/assets/logo.svg';
  logoAlt = 'Angular logo';
}
```

```ts
import { NgOptimizedImage } from '@angular/common';

@Component({
  template: `
    <img ngSrc="/assets/logo.svg" alt="Angular logo" width="32" height="32" />
    <img [ngSrc]="logoUrl" [alt]="logoAlt" width="32" height="32" />
    <img ngSrc="www.example.com/image.png" height="600" width="800" />
  `,
  imports: [NgOptimizedImage]
})
export class UserComponent {
  logoUrl = '/assets/logo.svg';
  logoAlt = 'Angular logo';
}
```

```ts
import { NgOptimizedImage } from '@angular/common';

@Component({
  template: `
    <img ngSrc="/assets/logo.svg" alt="Angular logo" width="32" height="32" />
    <img [ngSrc]="logoUrl" [alt]="logoAlt" width="32" height="32" />
    <img ngSrc="www.example.com/image.png" height="600" width="800" priority />
  `,
  imports: [NgOptimizedImage]
})
export class UserComponent {
  logoUrl = '/assets/logo.svg';
  logoAlt = 'Angular logo';
}
```

```ts
import { NgOptimizedImage } from '@angular/common';

@Component({
  template: `
    <img ngSrc="/assets/logo.svg" alt="Angular logo" width="32" height="32" />
    <img [ngSrc]="logoUrl" [alt]="logoAlt" width="32" height="32" />
    <img ngSrc="www.example.com/image.png" height="600" width="800" priority />
  `,
  imports: [NgOptimizedImage],
  providers: [
    provideImgixLoader('https://my.base.url/')
  ]
})
export class UserComponent {
  logoUrl = '/assets/logo.svg';
  logoAlt = 'Angular logo';
}
```

```ts
import { NgOptimizedImage } from '@angular/common';

// logo.svg e image.png será carregado de https://my.base.url/logo.svg e https://my.base.url/image.png

@Component({
  template: `
    <img ngSrc="logo.svg" alt="Angular logo" width="32" height="32" />
    <img [ngSrc]="logoUrl" [alt]="logoAlt" width="32" height="32" />
    <img ngSrc="image.png" height="600" width="800" priority />
  `,
  imports: [NgOptimizedImage],
  providers: [
    provideImgixLoader('https://my.base.url/')
  ]
})
export class UserComponent {
  logoUrl = 'logo.svg';
  logoAlt = 'Angular logo';
}
```


```ts
import { NgOptimizedImage } from '@angular/common';

// logo.svg e image.png será carregado de https://my.base.url/logo.svg e https://my.base.url/image.png
// Container div precisa ter 'position: "relative"'

@Component({
  template: `
    <img ngSrc="logo.svg" alt="Angular logo" width="32" height="32" />
    <img [ngSrc]="logoUrl" [alt]="logoAlt" width="32" height="32" />
    <img ngSrc="image.png" height="600" width="800" priority />
    <div class="image-container">
      <img ngSrc="image.png" fill />
    </div>
  `,
  imports: [NgOptimizedImage],
  providers: [
    provideImgixLoader('https://my.base.url/')
  ]
})
export class UserComponent {
  logoUrl = 'logo.svg';
  logoAlt = 'Angular logo';
}
```
````

---

# Enabling Routing

A maioria dos apps vão exigir navegarmos por mais de 1 single page. Para possibilitar isso,
precisamos do recurso de **Routing**.

````md magic-move
```ts
@Component({})
export class AppComponent {}
```

```ts
@Component({
  template: `
    <nav>
      <a href="/">Home</a> | <a href="/user">User</a>
    </nav>
  `
})
export class AppComponent {}
```

```ts
// Crie arquivo chamado app.routes.ts. Conteudo abaixo.
```

```ts
// Crie arquivo chamado app.routes.ts. Conteudo abaixo.
export const routes: Routes = [];
```

```ts
// Crie arquivo chamado app.routes.ts. Conteudo abaixo.
import {Routes} from '@angular/router';

export const routes: Routes = [];
```

```ts
// Crie arquivo chamado app.config.ts. Conteudo abaixo.
export const appConfig: ApplicationConfig = {
  providers: [],
};
```

```ts
// Crie arquivo chamado app.config.ts. Conteudo abaixo.
import { ApplicationConfig } from '@angular/core';

export const appConfig: ApplicationConfig = {
  providers: [],
};
```

```ts
// Crie arquivo chamado app.config.ts. Conteudo abaixo.
import { ApplicationConfig } from '@angular/core';

export const appConfig: ApplicationConfig = {
  providers: [provideRouter(routes)],
};
```

```ts
// Crie arquivo chamado app.config.ts. Conteudo abaixo.
import { ApplicationConfig } from '@angular/core';
import { routes } from './app.routes';
import { provideRouter } from '@angular/router';

export const appConfig: ApplicationConfig = {
  providers: [provideRouter(routes)],
};
```

```ts
// File app.component.ts

@Component({
  template: `
    <nav>
      <a href="/">Home</a> | <a href="/user">User</a>
    </nav>
  `
})
export class AppComponent {}
```

```ts
// File app.component.ts
import { RouterOutlet } from '@angular/router';

@Component({
  template: `
    <nav>
      <a href="/">Home</a> | <a href="/user">User</a>
    </nav>
  `,
  imports: [RouterOutlet],
})
export class AppComponent {}
```

```ts
// File app.component.ts
import { RouterOutlet } from '@angular/router';

@Component({
  template: `
    <nav>
      <a href="/">Home</a> | <a href="/user">User</a>
    </nav>
    <router-outlet />
  `,
  imports: [RouterOutlet],
})
export class AppComponent {}
```
````

---

## Definindo uma Rota

- Continuando a implementação anterior onde só habilitamos o Routing. Precisamos definir as rotas.
- Lembrando:
  - `/` : aponta para path `''` que carrega `HomeComponent`
  - `/user`: aponta para path `"user"` que carrega `UserComponent`

````md magic-move
```ts
// file app/home.component.ts
@Component({})
export class HomeComponent {}
```

```ts
// file app/home.component.ts
@Component({})
export class HomeComponent {}

// file app/user.component.ts
@Component({})
export class UserComponent {}
```

```ts
// file app/home.component.ts
@Component({
  selector: 'app-home',
  template: `
    <div>App Home Page</div>
  `
})
export class HomeComponent {}

// file app/user.component.ts
@Component({
  selector: 'app-user',
  template: `
    <div>App User Page</div>
  `
})
export class UserComponent {}
```

```ts
// file app.routes.ts.
import {Routes} from '@angular/router';

export const routes: Routes = [];
```

```ts
// file app.routes.ts.
import {Routes} from '@angular/router';

export const routes: Routes = [
  {
    path: '', // aponta para / no browser
    component: HomeComponent
  }
];
```

```ts
// file app.routes.ts.
import {Routes} from '@angular/router';
import {HomeComponent} from './home.component';

export const routes: Routes = [
  {
    path: '', // aponta para / no browser
    component: HomeComponent
  }
];
```

```ts
// file app.routes.ts.
import {Routes} from '@angular/router';
import {HomeComponent} from './home.component';

export const routes: Routes = [
  {
    path: '', // aponta para / no browser
    component: HomeComponent
  },
  {
    path: 'user', // aponta para /user no browser
    component: UserComponent
  }
];
```

```ts
// file app.routes.ts.
import {Routes} from '@angular/router';
import {HomeComponent} from './home.component';
import {UserComponent} from './user.component';

export const routes: Routes = [
  {
    path: '', // aponta para / no browser
    component: HomeComponent
  },
  {
    path: 'user', // aponta para /user no browser
    component: UserComponent
  }
];
```

```ts
// file app.routes.ts.
import {Routes} from '@angular/router';
import {HomeComponent} from './home.component';
import {UserComponent} from './user.component';

export const routes: Routes = [
  {
    path: '', // aponta para / no browser,
    title: 'App Home Page',
    component: HomeComponent
  },
  {
    path: 'user', // aponta para /user no browser
    title: 'App User Page',
    component: UserComponent
  }
];
```
````

---

## Linkar uma Rota com RouterLink
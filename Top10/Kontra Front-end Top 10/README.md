### Angular 1: Cross Site Request Forgery
**Descrição:** É possível utilizar o token de acesso de um outro usuário a partir de uma outra requisição.

**Vetor de ataque:** Pela falta de configurações de segurança o atacante consegue capturar o _token_ de acesso do outro usuário e realizar requisições no nome da vítima através do token de acesso, utilizando um _phising_ para capturar esse token.

**Prevenção:** Para prevenir esse tipo de ataque é utilizado um _header_ de segurança que impede o token de ser utilizado de uma outra requisição, samesite cookie, utilizar também o _X-Frame-Options_ e _token anti-CSRF_.

**Referência:** [Cross Site Request Forgery | Kontra (application.security)](https://application.security/free-application-security-training/angular-cross-site-request-forgery)

**Exemplo Vulnerável:** No exemplo abaixo o desenvolvedor não utiliza os parâmetros de segurança, _headers_ de segurança.
```javascript
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { HttpClientModule } from '@angular/common/http';
import { PhoneAPIService } from '../phone.api.service';

@Component({
  selector: 'phone-form',
  template: `
    <form [formGroup]="phoneForm" (ngSubmit)="onSubmit(phoneForm.value)">
      <label for="phone-input">Mobile Number</label>
      <input id="phone-input" name="phone" formControlName="phone">

      <button type="submit">Save</button>
      <button>Cancel</button>
    </form>
  `,
})
class PhoneFormComponent {
  phoneForm;

  constructor(private phoneAPIService: PhoneAPIService, private formBuilder: FormBuilder) {
    this.mobileForm = this.formBuilder.group({
      phone: '',
    });
  }

  onSubmit(data) {
    this.phoneAPIService.submit(data);
  }
}


@NgModule({
  imports: [
    BrowserModule,
    HttpClientModule,
  ],
  bootstrap: [PhoneFormComponent],
  declarations: [PhoneFormComponent],
})
export class AppModule {}
```

**Correção:** Para corrigir o código acima, o desenvolvedor colocou em sua requisição os _headers_ de segurança.
```javascript
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { HttpClientModule, HttpClientXsrfModule } from '@angular/common/http';
import { PhoneAPIService } from '../phone.api.service';

@Component({
  selector: 'phone-form',
  template: `
    <form [formGroup]="phoneForm" (ngSubmit)="onSubmit(phoneForm.value)">
      <label for="phone-input">Mobile Number</label>
      <input id="phone-input" name="phone" formControlName="phone">

      <button type="submit">Save</button>
      <button>Cancel</button>
    </form>
  `,
})
class PhoneFormComponent {
  phoneForm;

  constructor(private phoneAPIService: PhoneAPIService, private formBuilder: FormBuilder) {
    this.mobileForm = this.formBuilder.group({
      phone: '',
    });
  }

  onSubmit(data) {
    this.phoneAPIService.submit(data);
  }
}


@NgModule({
  imports: [
    BrowserModule,
    HttpClientModule,
    HttpClientXsrfModule.withOptions({
      cookieName: 'My-XSRF-Cookie',
      headerName: 'My-XSRF-Header',
    }),
  ],
  bootstrap: [PhoneFormComponent],
  declarations: [PhoneFormComponent],
})
export class AppModule {}
```


### Angular 2: Direct DOM Manipulation XSS
**Descrição:** A partir de um variável vulnerável o atacante consegue manipular o valor inserido para que seja executado um código _Javascript_.

**Vetor de ataque:** Utilizando a linguagem Javascript o atacante consegue inserir um código malicioso para roubar a sessão das vítimas, realizar um redirecionamento da página para um página _fake_ e roubar os dados da vítima.

**Prevenção:** Utilizar _headers_ de segurança, _X-XSS-Protection_, sanitizar dados enviados pelos usuários, sempre que possível negar a utilização de caracteres especiais entre outras.

**Referência:** [Direct DOM Manipulation XSS | Kontra (application.security)](https://application.security/free-application-security-training/angular-direct-dom-manipulation-xss)

**Exemplo Vulnerável:** O valor enviado é enviado diretamente para o DOM, não realizando nenhuma filtragem dos dados enviados.
```javascript
import { Component } from '@angular/core';

@Component({
  selector: 'tabs',
  template: `
    <h2>Find a Solution (<span id="currentTabName"></span>)</h2>

    <div class="tabs">
      <div class="tab">All</div>
      <div class="tab">Developer Tools</div>
      <div class="tab">Frameworks</div>
    </div>
  `,
})
export class TabsComponent {
  @Input() category: string;

  constructor() {
    this.category = new URL(location.href).searchParams.get('filter').replace('+', ' ') || 'All';
  }

  ngAfterViewInit() {
    document.getElementById('currentTabName').innerHTML = this.category;
  }
}
```

**Correção:** Utilizando _tag_ de interpolação caracteres especiais são sanitizados impedindo _scripts_ maliciosos.
```javascript
import { Component } from '@angular/core';

@Component({
  selector: 'tabs',
  template: `
    <h2>Find a Solution (<span>{{ category }}</span>)</h2>

    <div class="tabs">
      <div class="tab">All</div>
      <div class="tab">Developer Tools</div>
      <div class="tab">Frameworks</div>
    </div>
  `,
})
export class TabsComponent {
  @Input() category: string;

  constructor() {
    this.category = new URL(location.href).searchParams.get('filter').replace('+', ' ') || 'All';
  }
}
```


### Angular 3: Template Concatenation XSS
**Descrição:** A partir do _template_ _HTML_ é possível injetar códigos _Javascript_ para a execução de ações inesperadas pelo sistema.

**Vetor de ataque:** Utilizando o template default de alguns _frameworks_, é possível injetar códigos _HTML_ para conseguir informações sensíveis sobre outros usuários ou do próprio sistema.

**Prevenção:** Para prevenir esse tipo de ataque é recomendado tratar todos os dados enviados pelos usuários, realizando a sanitização dos dados, utilizando _headers_ de segurança e também sempre que possível impedir que o usuário utilize caracteres especiais.

**Referência:** [Template Concatenation XSS | Kontra (application.security)](https://application.security/free-application-security-training/angular-template-concatenation-xss)

**Exemplo Vulnerável:** Na codificação da página em Angular, o desenvolvedor injeta diretamente o código enviado pelo usuário sem realizar algum tratamento daquela informação.
```javascript
import { Component } from '@angular/core';

const searchQuery: string = (new URL(location.href)).searchParams.get('query');

@Component({
  selector: 'search-results',
  template: `
    <div class="search-results">
      <span>No results found for: ${searchQuery}</span>
    </div>
  `,
})
export class NoSearchResultsMessageComponent {}
```

**Correção:** Nesse código o desenvolvedor utilizar de interpolação de _tags_ isso faz com que o Angular transforme toda a informação em _string_, sanitizando e escapando códigos maliciosos.
```javascript
import { Component } from '@angular/core';

@Component({
  selector: 'search-results',
  template: `
    <div class="search-results">
      <span>No results found for: {{ searchQuery }}</span>
    </div>
  `,
})
export class NoSearchResultsMessageComponent {
  @Input() searchQuery: string;

  constructor() {
    this.searchQuery = new URL(location.href).searchParams.get('query');
  }
}
```


### Angular 4: Sanitization Misuse XSS
**Descrição:** Utilizar features que permitam que o sistema reconheça exatamente o que o usuário está enviando pode gerar falhas no sistema.

**Vetor de ataque:** Quando um usuário pode enviar qualquer tipo de dado que o servidor vai tratar, gera um vetor de ataque onde o atacante utilizando código _Javascript_ para obter sessões de outros usuários ou dados sensíveis.

**Prevenção:** Para evitar esse tipo de ataque, o ideal é sempre validar os dados que o usuário está enviando, evitar sempre que possível utilizar features que permitam que o usuário consiga enviar todo tipo de dado para o servidor sem uma sanitização.

**Referência:** [Sanitization Misuse XSS | Kontra (application.security)](https://application.security/free-application-security-training/angular-sanitization-misuse-xss)

**Exemplo Vulnerável:** Utilizar o "_sanitizer.bypassSecurityTrustHtml(ticket.description)_" faz com que o Angular não consiga realizar a sanitização dos dados enviados pelos usuários.
```javascript
import { Component } from '@angular/core';
import { DomSanitizer, SafeHtml } from '@angular/platform-browser';

@Component({
  selector: 'support-ticket',
  template: `
    <div>
      <h2>Details</p>
      <p>Type: {{ ticket.type }}</p>
      <p>Status: {{ ticket.status }}</p>
      <p>Priority: {{ ticket.priority }}</p>
      <p>Resolution: {{ ticket.resolution }}</p>
    </div>

    <div>
      <h2>People</p>
      <p>Assignee: {{ ticket.assignee }}</p>
      <p>Reporter: {{ ticket.reporter }}</p>
    </div>

    <div>
      <h2>Description</p>
      <p [innerHTML]="richDescription"><p> 
    </div>
  `,
})
export class SupportTicketComponent {
  @Input() ticket: object;
  richDescription: SafeHtml;

  constructor(private sanitizer: DomSanitizer) {
    this.richDescription = sanitizer.bypassSecurityTrustHtml(ticket.description);
  }
}
```

**Correção:** Removendo o "_sanitizer.bypassSecurityTrustHtml(ticket.description)_" o Angular consegue limpar a entrada de dados removendo _scripts_ maliciosos, mantendo apenas _tags_ seguras, _strong_ e _i_.
```javascript
import { Component } from '@angular/core';

@Component({
  selector: 'support-ticket',
  template: `
    <div>
      <h2>Details</p>
      <p>Type: {{ ticket.type }}</p>
      <p>Status: {{ ticket.status }}</p>
      <p>Priority: {{ ticket.priority }}</p>
      <p>Resolution: {{ ticket.resolution }}</p>
    </div>

    <div>
      <h2>People</p>
      <p>Assignee: {{ ticket.assignee }}</p>
      <p>Reporter: {{ ticket.reporter }}</p>
    </div>

    <div>
      <h2>Description</p>
      <p [innerHTML]="ticket.description"><p> 
    </div>
  `,
})
export class SupportTicketComponent {
  @Input() ticket: object;
}
```


### React 1: Cross Site Request Forgery
**Descrição:** Sequestro de _token_, quando o token não é renovado e o atacante consegue capturar o _token_ de sessão de outro usuário.

**Vetor de ataque:** A partir dessa falha o atacante gera um script se passando pelo site para conseguir roubar o _token_ de acesso do usuário, com isso consegue realizar requisições se passando por aquele usuário.

**Prevenção:** Utilizar _HttpOnly_, _Samesite_, _headers_ de segurança como, _X-Frame-Options_ e um _token anti-CSRF_.

**Referência:** [Cross Site Request Forgery | Kontra (application.security)](https://application.security/free-application-security-training/react-cross-site-request-forgery)

**Exemplo Vulnerável:** Para realizar a requesição o desenvolvedor não está utilizando _headers_ de segurança, com isso caso o atacante tenha roubado o _token_ da vítima o fraudador consegue realizar requisições no nome da vítima.
```javascript
import React, { useState } from 'react';

const PhoneForm = () => {
  const [phone, setPhone] = useState('');

  const onChange = ({ target }) => setPhone(target.value);

  const onSubmit = () => {
    fetch('https://www.sparkpay.com/api/phone', {
      method: 'PUT',
      credentials: 'include',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({
        phone,
      }),
    });
  };

  return (
    <form name="phoneForm" onSubmit={onSubmit}>
      <label for="phone-input">Mobile Number</label>
      <input id="phone-input" name="phone" value={phone} onChange={onChange}>

      <button type="submit">Save</button>
      <button>Cancel</button>
    </form>
  );
};
```

**Correção:** Nesse segundo código o desenvolvedor está utilizando os _headers_ precavendo que o atacante realize uma requisição no nome da vítima.
```javascript
import React, { useState } from 'react';

const PhoneForm = () => {
  const [phone, setPhone] = useState('');

  const onChange = ({ target }) => setPhone(target.value);

  const onSubmit = () => {
    const csrfToken = document.querySelector('meta[name="csrf-token"]').getAttribute('content');

    fetch('https://www.sparkpay.com/api/phone', {
      method: 'PUT',
      credentials: 'include',
      headers: {
        'Content-Type': 'application/json',
        'X-CSRF-TOKEN': csrfToken,
      },
      body: JSON.stringify({
        phone: phone,
      }),
    });
  };

  return (
    <form name="phoneForm" onSubmit={onSubmit}>
      <label for="phone-input">Mobile Number</label>
      <input id="phone-input" name="phone" value={phone} onChange={onChange}>

      <button type="submit">Save</button>
      <button>Cancel</button>
    </form>
  );
};
```


### React 2: Direct DOM Manipulation XSS
**Descrição:** Utilizando de um parâmetro o atacante injeta um código malicioso tendo várias finalidades.

**Vetor de ataque:** A partir de um parâmetro de _query_, variável, o atacante injeta um código _Javascript_, podendo assim roubar sessão de outro usuário ou informações sensíveis.

**Prevenção:** Utilizar _headers_ de segurança, _X-XSS-Protection_, se possível não disponibilizar caracteres especiais para os usuários, realizando uma sanitização dos dados.

**Referência:** [Direct DOM Manipulation XSS | Kontra (application.security)](https://application.security/free-application-security-training/react-direct-dom-manipulation-xss)

**Exemplo Vulnerável:** No código abaixo o desenvolvedor está passando os dados enviados pelo usuário direto para o DOM, permitindo assim enviar _scripts_ maliciosos.
```javascript
import React, { useEffect } from 'react';

const Tabs = () => {
  useEffect(() => {
    const category = new URL(location.href).searchParams.get('filter').replace('+', ' ') || 'All';
    document.getElementById('currentTabName').innerHTML = category; // <span id="currentTabName">{{filter}}</span>
  }, []);

  return (
    <div>
      <h2>Find a Solution (<span id="currentTabName"></span>)</h2>

      <div className="tabs">
        <div>All</div>
        <div>Developer Tools</div>
        <div>Frameworks</div>
      </div>
    </div>
  );
};
```

**Correção:** Utilizando a interpolação de _tags_ do React, é possível escapar caracteres de marcação convertendo diretamente em _string_.
```javascript
import React, { useState, useEffect } from 'react';

const Tabs = () => {
  const [category, setCategory] = useState('All');

  useEffect(() => {
    const category = new URL(location.href).searchParams.get('filter').replace('+', ' ') || 'All';
    setCategory(category);
  }, []);

  return (
    <div>
      <h2>Find a Solution (<span>{category}</span>)</h2>

      <div className="tabs">
        <div>All</div>
        <div>Developer Tools</div>
        <div>Frameworks</div>
      </div>
    </div>
  );
};
```


### React 3: Components with Known Vulnerabilities
**Descrição:** Componentes com vulnerabilidades conhecidas gera vetores de ataque para os fraudadores.

**Vetor de ataque:** Utilizando de uma vulnerabilidade conhecida, o atacante utiliza dessa documentação para prejudicar o serviço da vítima, sendo usuário ou servidor.

**Prevenção:** Para que as vulnerabilidades sejam corrigidas as empresas realizam atualizações nos seus serviços, por isso manter seu sistema o mais atualizado possível diminui as quantidades de vetores de ataque.

**Referência:** [Components with Known Vulnerabilities | Kontra (application.security)](https://application.security/free-application-security-training/react-components-with-known-vulnerabilities)

**Exemplo Vulnerável:** Utilizar componentes desatualizados permiti abusar das falhas já registradas.
```javascript
<!DOCTYPE html>
<html lang="en">
<head>
  <title>Legion Y7000 | 15.6' gaming laptop | Lenovo UK</title>
  <meta charset="utf-8">
  <meta http-equiv="X-UA-Compatible" content="IE=10">
  <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

  <script src="/assets/app.js"></script>
</head>

<body>
  <div class="Header">...</div>

  <div class="ProductImageContainer">
    <div class="react-pretty-photo">
      <img src="/assets/images/preview.png" alt="Product Preview">
    </div>
  </div>

  <div class="Footer">...</div>
</body>
```


### React 4: Untrusted HTML Rendering XSS
**Descrição:** Essa vulnerabilidade consiste em apresentar o código _HTML_ na página, isso permite que o atacante injete um código malicioso prejudicando o sistema.

**Vetor de ataque:** Em alguns momentos o sistema tem que apresentar um código _HTML_, porém o atacante pode utilizar dessa falha para conseguir injetar um código malicioso e roubar sessão de outro usuário, dados sensíveis entre outras informações.

**Prevenção:** Sempre sanitizar as informações que os usuários estão enviando para o servidor, mesmo que seja necessário sempre filtrar o que o usuário pode enviar, utilizando sempre também _headers_ de segurança, _X-XSS-Protection_.

**Referência:** [Untrusted HTML Rendering XSS | Kontra (application.security)](https://application.security/free-application-security-training/react-untrusted-html-rendering-xss)

**Exemplo Vulnerável:** Para que o React não escape os valores, os desenvolvedores utilizam _dangerouslySetInnerHTML_ para seja enviado exatamente o que o usuário enviou.
```javascript
import React from 'react';
import PropTypes from 'prop-types';

const SupportTicket = ({ ticket }) => {
  return (
    <div>
      <div>
        <h2>Details</p>
        <p>Type: {ticket.type}</p>
        <p>Status: {ticket.status}</p>
        <p>Priority: {ticket.priority}</p>
        <p>Resolution: {ticket.resolution}</p>
      </div>

      <div>
        <h2>People</p>
        <p>Assignee: {ticket.assignee}</p>
        <p>Reporter: {ticket.reporter}</p>
      </div>

      <div>
        <h2>Description</p>
        <p dangerouslySetInnerHTML={{ __html: ticket.description }} /> 
      </div>
    </div>
  );
};

SupportTicket.propTypes = {
  ticket: PropTypes.object.isRequired,
};
```

**Correção:** Para corrigir a falha o desenvolvedor importa a biblioteca sanitizeHtml, com isso ele retira _tags_ como _Javascript_, deixando apenas as _tags_ seguras como, _strong_ e _em_.
```javascript
import React from 'react';
import PropTypes from 'prop-types';
import sanitizeHtml from 'sanitize-html';

const SupportTicket = ({ ticket }) => {
  const sanitizedDescription = sanitizeHtml(ticket.description);

  return (
    <div>
      <div>
        <h2>Details</p>
        <p>Type: {ticket.type}</p>
        <p>Status: {ticket.status}</p>
        <p>Priority: {ticket.priority}</p>
        <p>Resolution: {ticket.resolution}</p>
      </div>

      <div>
        <h2>People</p>
        <p>Assignee: {ticket.assignee}</p>
        <p>Reporter: {ticket.reporter}</p>
      </div>

      <div>
        <h2>Description</p>
        <p dangerouslySetInnerHTML={{ __html: sanitizedDescription }} /> 
      </div>
    </div>
  );
};

SupportTicket.propTypes = {
  ticket: PropTypes.object.isRequired,
};
```


### Vue.js 1: Untrusted HTML Rendering XSS
**Descrição:** O atacante abusa de campos que recebem valores inseridos pelos usuário, utilizando de códigos _Javascript_ maliciosos.

**Vetor de ataque:** Após analisar o _HTML_ o atacante identifica um campo que recebe um valor do usuário, com isso ele escreve um código malicioso e envia para o sistema, tendo o objetivo de roubar sessões, dados entre outros.

**Prevenção:** Utilizar ferramentas e métodos que realizem a sanitização dos dados enviados pelos usuários, sempre que possível também impedir a utilização de caracteres especiais.

**Referência:** [Untrusted HTML Rendering XSS | Kontra (application.security)](https://application.security/free-application-security-training/vue-untrusted-html-rendering-xss)

**Exemplo Vulnerável:** Para que substituir o Vue, o desenvolvedor utilza o v-html fazendo com que os dados enviados pelos usuários sejam enviados direto para o DOM.
```javascript
const SupportTicket = new Vue({
  template: `
    <div>
      <div>
        <h2>Details</p>
        <p>Type: {{ ticket.type }}</p>
        <p>Status: {{ ticket.status }}</p>
        <p>Priority: {{ ticket.priority }}</p>
        <p>Resolution: {{ ticket.resolution }}</p>
      </div>

      <div>
        <h2>People</p>
        <p>Assignee: {{ ticket.assignee }}</p>
        <p>Reporter: {{ ticket.reporter }}</p>
      </div>

      <div>
        <h2>Description</p>
        <p v-html="ticket.description"></p> 
      </div>
    </div>
  `,

  props: {
    ticket: ['ticket'],
  },
});
```

**Correção:** Para corrigir a vulnerabilidade o desenvolvedor importou a biblioteca sanitizeHtml, fazendo assim uma sanitização dos dados e deixando apenas as _tags_ seguras como, _strong_ e _em_.
```javascript
import sanitizeHtml from 'sanitize-html';

const SupportTicket = new Vue({
  template: `
    <div>
      <div>
        <h2>Details</p>
        <p>Type: {{ ticket.type }}</p>
        <p>Status: {{ ticket.status }}</p>
        <p>Priority: {{ ticket.priority }}</p>
        <p>Resolution: {{ ticket.resolution }}</p>
      </div>

      <div>
        <h2>People</p>
        <p>Assignee: {{ ticket.assignee }}</p>
        <p>Reporter: {{ ticket.reporter }}</p>
      </div>

      <div>
        <h2>Description</p>
        <p v-html="sanitizeHtml(ticket.description)"></p> 
      </div>
    </div>
  `,

  props: {
    ticket: ['ticket'],
  },
});
```


### Vue.js 2: Direct DOM Manipulation XSS
**Descrição:** Um código malicioso injetado no código do serviço a partir de um campo onde o usuário envia dados.

**Vetor de ataque:** Utilizando desse campo que o usuário pode enviar dados, o atacante hospeda um código malicioso com o intuito de roubar informações, sessões entre outros.

**Prevenção:** Utilizar os _headers_ de segurança, sempre sanitizar os dados enviado pelos usuários e sempre que possível impedir que os usuários possam utilizar caracteres especiais.

**Referência:** [Direct DOM Manipulation XSS | Kontra (application.security)](https://application.security/free-application-security-training/vue-direct-dom-manipulation-xss)

**Exemplo Vulnerável:** Para a execução do código abaixo o desenvolvedor está utilizando o valor que o usuário envia diretamente para o DOM sem tratamento, deixando possível enviar _scripts_ maliciosos.
```javascript
const Tabs = new Vue({
  template: `
    <div>
      <h2>Find a Solution (<span id="currentTabName"></span>)</h2>

      <div className="tabs">
        <div>All</div>
        <div>Developer Tools</div>
        <div>Frameworks</div>
      </div>
    </div>
  `,

  mounted: function () {
    const category = new URL(location.href).searchParams.get('filter').replace('+', ' ') || 'All';
    document.getElementById('currentTabName').innerHTML = category;
  }
});
```

**Correção:** Utilizando a interpolação de _tags_ do Vue, é possível sanitizar os dados enviados pelos usuários, fazendo escapar alguns caracteres especiais.
```javascript
const Tabs = new Vue({
  template: `
    <div>
      <h2>Find a Solution (<span>{{ category }}</span>)</h2>

      <div className="tabs">
        <div>All</div>
        <div>Developer Tools</div>
        <div>Frameworks</div>
      </div>
    </div>
  `,

  data: {
    category: new URL(location.href).searchParams.get('filter').replace('+', ' ') || 'All',
  },
});
```


### Vue.js 3: Cross Site Request Forgery
**Descrição:** A partir dessa vulnerabilidade o atacante rouba a sessão do usuário, o _token_ de acesso, com isso ele consegue realizar requisições pela vítima. 

**Vetor de ataque:** O atacante replica o site com um domínio diferente, com isso ele cria um e-mail de isca, _phishing_, para que a vítima enviei seu token para sem o seu servidor, com isso ele consegue realizar requisições no nome da vítima conseguindo informações ou alterando informações.

**Prevenção:** Para evitar o ataque é necessário implementar _tokens anti-CSRF_, _headers_ de segurança _X-Frame-Options_, utilizar _Samesite_ e _HttpOnly_ no _cookies_ entre outras.

**Referência:** [Cross Site Request Forgery | Kontra (application.security)](https://application.security/free-application-security-training/vue-cross-site-request-forgery)

**Exemplo Vulnerável:** No exemplo abaixo a requisição é feita sem os _headers_ de segurança, sendo possível o atacante requisitarem para outros usuários com o token da vítima.
```javascript
<template>
  <form name="phoneForm" @submit="submitForm">
    <label for="phone-input">Mobile Number</label>
    <input id="phone-input" name="phone" v-model="phone">

    <button type="submit">Save</button>
    <button>Cancel</button>
  </form>
</template>

<script>
  module.exports = {
    data: {
      phone: '',
    },

    methods: {
      submitForm: function () {
        fetch('https://www.sparkpay.com/api/phone', {
          method: 'PUT',
          credentials: 'include',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify({
            phone: this.phone,
          }),
        });
      }
    },
  };
</script>
```

**Correção:** Para que não seja possível executar requisições pela vítima, o desenvolvedor utiliza os _headers_ de segurança fazendo assim que mesmo que o atacante tenha o _token_ não consiga realizar a requisição.
```javascript
<template>
  <form name="phoneForm" @submit="submitForm">
    <label for="phone-input">Mobile Number</label>
    <input id="phone-input" name="phone" v-model="phone">

    <button type="submit">Save</button>
    <button>Cancel</button>
  </form>
</template>

<script>
  module.exports = {
    data: {
      phone: '',
    },

    methods: {
      submitForm: function () {
        const csrfToken = document.querySelector('meta[name="csrf-token"]').getAttribute('content');

        fetch('https://www.sparkpay.com/api/phone', {
          method: 'PUT',
          credentials: 'include',
          headers: {
            'Content-Type': 'application/json',
            'X-XSRF-TOKEN': csrfToken,
          },
          body: JSON.stringify({
            phone: this.phone,
          }),
        });
      }
    },
  };
</script>
```


### Vue.js 4: Untrusted Template Usage XSS
**Descrição:** Alguns padrões de _template_ possibilitam que os usuários enviem códigos maliciosos.

**Vetor de ataque:** Utilizando esses códigos maliciosos os atacantes tentam roubar informações, sessões entre outras.

**Prevenção:** Utilizar _headers_ de segurança, sempre sanitizar os dados dos usuários e se possível impedir a utilização de caracteres especiais, avaliar as funcionalidades default dos _frameworks_ para que não exponha seu serviço.

**Referência:** [Untrusted Template Usage XSS | Kontra (application.security)](https://application.security/free-application-security-training/vue-untrusted-template-usage-xss)

**Exemplo Vulnerável:** Utilizando o _framework_ do Vue para criar dinamicamente o HTML, sem utilizar algum método para sanitizar os dados a aplicação está vulneravel já que os dados são enviados para o servidor exatamente como o usuário enviou.
```javascript
const searchQuery = new URL(location.href).searchParams.get('query');

new Vue({
  template: `
    <div class="search-results">
      <span>No results found for: ${searchQuery}</span>
    </div>
  `,
});
```

**Correção:** A interpolação de _strings _tags_ do Vue, permite que escape alguns caracteres especiais, realizando uma sanitização e transformando todos os dados enviados pelo usuário em _string_.
```javascript
new Vue({
  template: `
    <div class="search-results">
      <span>No results found for: {{ searchQuery }}</span>
    </div>
  `,
  
  data: {
    searchQuery: new URL(location.href).searchParams.get('query'),
  },
});
```

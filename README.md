<p align="center">
  <a href="https://sweetalert2.github.io">
    <img src="ngx-sweetalert2-logo.png" alt="SweetAlert2">
  </a>
</p>

<h1 align="center">@sweetalert2/ngx-sweetalert2</h1>

<p align="center">
  Official <a href="https://sweetalert2.github.io">SweetAlert2</a> integration for Angular
</p>

<p align="center">
    <a href="https://www.npmjs.com/package/@sweetalert2/ngx-sweetalert2"><img alt="npm version" src="https://img.shields.io/npm/v/@sweetalert2/ngx-sweetalert2.svg?style=flat-square"></a>
    <a href="https://github.com/sweetalert2/ngx-sweetalert2/actions"><img alt="Build Status" src="https://github.com/sweetalert2/ngx-sweetalert2/workflows/build/badge.svg"></a>
    <a href="LICENSE"><img alt="license" src="https://img.shields.io/github/license/sweetalert2/ngx-sweetalert2.svg?style=flat-square"></a>
</p>

<br>

This is not a regular API wrapper for SweetAlert (which already works very well alone), it intends to provide Angular-esque utilities on top of it.

:point_right: **Before posting an issue**, please check that the problem isn't on SweetAlert's side.

----------------

### Quick start

 - [Installation & Usage](#package-installation--usage)
 - [`[swal]` directive](#swaldirective) — for simple, one-liner dialogs
 - [`<swal>` component](#swalcomponent) — for advanced use cases and extended Swal2 API coverage
 - [`*swalPortal` directive](#swalportaldirective) — use Angular templates in `<swal>`

### Wiki recipes

 - 🔌 [All possible ways of firing a modal and controlling its visibility](https://github.com/sweetalert2/ngx-sweetalert2/wiki/All-possible-ways-of-firing-a-modal-and-controlling-its-visibility)
 - :wrench: [Setting global defaults (SweetAlert2 mixins)](https://github.com/sweetalert2/ngx-sweetalert2/wiki/Setting-global-defaults-(SweetAlert2-mixins))
 - :art: [Use a theme from @sweetalert2/themes (and or customize SCSS variables)
](https://github.com/sweetalert2/ngx-sweetalert2/wiki/Use-a-theme-from-@sweetalert2-themes-(and-or-customize-SCSS-variables))

----------------

## :package: Installation & Usage

1) Install _ngx-sweetalert2_ and _sweetalert2_ via the npm registry:

```sh
npm install sweetalert2 @sweetalert2/ngx-sweetalert2
```

:arrow_double_up: Always upgrade SweetAlert2 when you upgrade ngx-sweetalert2. The latter is statically linked with SweetAlert2's type definitions.

## Angular and SweetAlert2 versions compatibility table

| Angular version | Latest compatible version range                                                                                                                          | Required SweetAlert2 version range |
|-----------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------|
| Angular 18+     | @sweetalert2/ngx-sweetalert2@**^13.0.0** (current)                                                                                                       | sweetalert2@**^11.0.0**            |
| Angular 14-17   | [@sweetalert2/ngx-sweetalert2@**^12.0.0**](https://github.com/sweetalert2/ngx-sweetalert2/tree/v12.0.0#readme)                                           | sweetalert2@**^11.0.0**            |
| Angular 12, 13  | [@sweetalert2/ngx-sweetalert2@**^11.0.0**](https://github.com/sweetalert2/ngx-sweetalert2/tree/v11.0.0#readme)                                           | sweetalert2@**^11.0.0**            |
| Angular 9 to 11 | [@sweetalert2/ngx-sweetalert2@**~9.0.0**](https://github.com/sweetalert2/ngx-sweetalert2/tree/v9.0.0#readme)                                             | sweetalert2@**^10.8.0**            |
| Angular 8       | [@sweetalert2/ngx-sweetalert2@**~7.3.0**](https://github.com/sweetalert2/ngx-sweetalert2/tree/v7.3.0#readme) (:warning: NOT ~7.4.0, broken AoT metadata) | sweetalert2@**^9.7.0**             |
| Angular 7       | [@sweetalert2/ngx-sweetalert2@**^5.1.0**](https://github.com/sweetalert2/ngx-sweetalert2/tree/v5.1.0#readme)                                             | sweetalert2@**^8.5.0**             |
| Angular 6       | [@sweetalert2/ngx-sweetalert2@**^5.1.0**](https://github.com/sweetalert2/ngx-sweetalert2/tree/v5.1.0#readme)                                             | sweetalert2@**^8.5.0**             |
| Angular 5       | [@sweetalert2/ngx-sweetalert2@**^5.1.0**](https://github.com/sweetalert2/ngx-sweetalert2/tree/v5.1.0#readme)                                             | sweetalert2@**^8.5.0**             |
| Angular 4       | [@toverux/ngx-sweetalert2@**^3.4.0**](https://github.com/sweetalert2/ngx-sweetalert2/tree/v3.4.0#readme)                                                 | sweetalert2@**^7.15.1**            |
| Angular 2       | Try Angular 4 versions requirements, or older versions like @toverux/ngsweetalert2                                                                       | unknown                            |

2) Import the module:

```typescript
import { SweetAlert2Module } from '@sweetalert2/ngx-sweetalert2';

@NgModule({
    //=> Basic usage (forRoot can also take options, see the wiki)
    imports: [SweetAlert2Module.forRoot()],

    //=> In submodules only:
    imports: [SweetAlert2Module],

    //=> In submodules only, overriding options from your root module:
    imports: [SweetAlert2Module.forChild({ /* options */ })]
})
export class AppModule {
}
```

That's it! By default, SweetAlert2 will be lazy-loaded, only when needed, from your local dependency of `sweetalert2`, using the `import()` syntax under the hood.

## :link: API

### `SwalDirective`

Add the `[swal]` attribute to an element to show a simple modal when that element is clicked.

To define the modal contents, you can pass a [`SweetAlertOptions` (provided by sweetalert2)](https://github.com/sweetalert2/sweetalert2/blob/main/sweetalert2.d.ts) object,
or a simple array of strings, of format `[title: string, text: string (, icon: string)]`.

A simple dialog:

```html
<button [swal]="['Oops!', 'This is not implemented yet :/', 'warning']">
  Do it!
</button>
```

More advanced, with text input, confirmation, denial and dismissal handling:

```html
<button
  [swal]="{ title: 'Save file as...', input: 'text', showDenyButton: true, denyButtonText: 'Don\'t save', showCancelButton: true }"
  (confirm)="saveFile($event)"
  (deny)="handleDenial()"
  (dismiss)="handleDismiss($event)">

  Save
</button>
```

```typescript
export class MyComponent {
  public saveFile(fileName: string): void {
    // ... save file
  }

  public handleDenial(): void {
      // ... don't save file and quit
  }

  public handleDismiss(dismissMethod: string): void {
    // dismissMethod can be 'cancel', 'overlay', 'close', and 'timer'
    // ... do something
  }
}
```

The directive can also take a reference to a [`<swal>` component](#swalcomponent) for more advanced use cases:

```html
<button [swal]="deleteSwal" (confirm)="deleteFile(file)">
  Delete {{ file.name }}
</button>

<swal #deleteSwal title="Delete {{ file.name }}?" etc></swal>
```

### `SwalComponent`

The library also provides a component, that can be useful for advanced use cases, or when you `[swal]`
has too many options.

The component also allows you to use Angular dynamic templates inside the SweetAlert (see the
[`*swalPortal` directive](#swalportaldirective) for that).

Simple example:

```html
<swal
  #deleteSwal
  title="Delete {{ file.name }}?"
  text="This cannot be undone"
  icon="question"
  [showCancelButton]="true"
  [focusCancel]="true"
  (confirm)="deleteFile(file)">
</swal>

With [swal]:
<button [swal]="deleteSwal">Delete {{ file.name }}</button>

Or DIY:
<button (click)="deleteSwal.fire()">Delete {{ file.name }}</button>
```

You can access the dialog from your TypeScript code-behind like this:

```ts
class MyComponent {
  @ViewChild('deleteSwal')
  public readonly deleteSwal!: SwalComponent;
}
```

You can pass native SweetAlert2 options via the `swalOptions` input, just in the case you need that:

```html
<swal [swalOptions]="{ confirmButtonText: 'I understand' }"></swal>
```

By the way: every "special" option, like `swalOptions`, that are not native options from SweetAlert2,
are prefixed with `swal`.

You can catch other modal lifecycle events than (confirm), (deny) or (cancel):

```html
<swal
  (willOpen)="swalWillOpen($event)"
  (didOpen)="swalDidOpen($event)"
  (didRender)="swalDidRender($event)"
  (willClose)="swalWillClose($event)"
  (didClose)="swalDidClose()"
  (didDestroy)="swalDidDestroy()">
</swal>
```

```typescript
export class MyComponent {
    public swalWillOpen(event: WillOpenEvent): void {
      // Most events (those using $event in the example above) will let you access the modal native DOM node, like this:
      console.log(event.modalElement);
    }
}
```

### `SwalPortalDirective`

The `*swalPortal` structural directive lets you use Angular dynamic templates inside SweetAlerts.

The name "portal" is inspired by React or Angular CDK portals.
The directive will replace certain parts of the modal (aka. _swal targets_) with embedded Angular views.

This allows you to have data binding, change detection, and use every feature of the Angular template syntax
you want, just like if the SweetAlert was a normal Angular component (it's not at all).

```html
<swal title="SweetAlert2 Timer">
  <div *swalPortal class="alert alert-info">
    <strong>{{ elapsedSeconds }}</strong> seconds elapsed since the modal was opened.
  </div>
</swal>
```

Using a structural directives allows us to take your content as a template, instantiate it lazily when needed
(i.e. when the modal is shown), and putting it in a native DOM element that is originally outside the scope of
your Angular app.

In this example we set the main content of the modal, where the `text` property is usually rendered when SweetAlert2
is in charge.
You can also target the title, the footer, or even the confirm button, and more!

You just have to change the _target_ of the portal (_`content`_ is the default target).
First, inject this little service in your component:

```typescript
import { SwalPortalTargets } from '@sweetalert2/ngx-sweetalert2';

export class MyComponent {
  public constructor(public readonly swalTargets: SwalPortalTargets) {
  }
}
```

Then, set the appropriate target as the value of `*swalPortal`, here using two portals, the first one
targeting the modal's content (this is the default), and the other one targeting the confirm button text.

```html
<swal title="Fill the form, rapidly" (confirm)="sendForm(myForm.value)">
  <!-- This form will be displayed as the alert main content
       Targets the alert's main content zone by default -->
  <form *swalPortal [formControl]="myForm">
    ...
  </form>

  <!-- This targets the confirm button's inner content
       Notice the usage of ng-container to avoid creating an useless DOM element inside the button -->
  <ng-container *swalPortal="swalTargets.confirmButton">
    Send ({{ secondsLeft }} seconds left)
  </ng-container>
</swal>
```

We have the following targets: `closeButton`, `title`, `content`, `actions`, `confirmButton`, `cancelButton`, and `footer`.

These targets are mostly provided by SweetAlert2 and made available in the right format for swal portals by
this library, but you can also make your own if you need to (take inspiration from the original service source).
Those are just variables containing a function that returns a modal DOM element, not magic.
The magic is inside the directive ;)

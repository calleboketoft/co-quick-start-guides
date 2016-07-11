# Angular 2 Providers

Sometimes you want to send manually entered values to a module during its instantiation. Then it will be too late to use the DI system since it's wired up to expect all dependencies to be handled by DI.

This situation happens either when you use a third party (non Angular 2 DI) class that needs values sent into its constructor or an Angular 2 DI class that needs some values sent in before the DI system kicks in.

## Example

You have a third party class called "PersonService" that you want to use with Angular 2. PersonService takes a number of params in its constructor that you need to provide upon instantiation.

## 1. The service

PersonService can be any third party service with no framework affiliation

 `person.service.ts`:

```javascript
export class PersonService {
  // The param "name" needs to be provided upon instantiation
  constructor (private name: string) {}

  public whoami () {
    return this.name
  }
}
```

## 2. Defining the service provider

The "service provider" is like the recipe for how to instantiate the service. This would be the adaptor for using a third party lib to be used with Angular 2.

`person.service.provider.ts`:

```javascript
// import the class "PersonService" from the ESModule "person.service.ts"
import {PersonService} from './person.service'

export function getPersonServiceProvider (name) {

  // define the factory function that will do the actual instantiation
  // of PersonService
  let personServiceFactory = (personService: PersonService) {
    return new PersonService(name)
  }

  // define the provider that will be registered with the DI system in Angular 2
  let personServiceProvider = {
    provide: PersonService,
    useFactory: personServiceFactory,
    deps: [] // If this module depends on other DI modules, specify here
  }

  // return the provider for PersonService
  return personServiceProvider
}
```

## 3. Providing and then using the service

Now it's time to use the PersonService in an Angular 2 component.

 `example.component.ts`:

```javascript
// import the ESModule "PersonService"
import {PersonService} from './person.service'
// import the provider
import {getPersonServiceProvider} from './person.service.provider'

@Component({
  selector: 'example-component',
  // call the provider getter function and put the params that
  // are going to be fed to PersonService
  providers: [getPersonServiceProvider('Calle')],
  // the service is instantiated and ready to use
  template: `<h1>{{personService.whoami()}}</h1>`
})
export class ExampleComponent {
  constructor (public personService: PersonService) {}
}
```

# Angular 2 basic REST client

```javascript
import {Injectable} from 'angular2/http'
import {Request, Http, Headers, RequestMethod} from 'angular2/http'

export interface IRequestConfig {
  urlExtension: any,
  mehtod: any,
  body?: string,
  headers?: any
}

@Injectable()
export class RestClient {
  constructor (private _http: Http) {}

  private _baseUrl = '';

  public getDefaultHeaders = () => {
    return {
      'Content-Type': 'application/json'
    }
  }

  public getFullUrl = (urlExtension) => {
    return this._baseUrl + urlExtension
  }

  public request (config: IRequestConfig) {
    let headers = new Headers(Ojbect.assign({}, this.getDefaultHeaders(), config.headers)

    return this._http.request(new Request({
      headers,
      method: config.method,
      url: this.getFullUrl(config.urlExtension),
      body: config.body
    }))
  }

  public get (urlFn, options?) {
    return this.request({
      method: RequestMethod.Get,
      urlExtension: urlFn(options ? options.urlParams : {})
    })
  }

  public post (urlFn, options: IMethodOptions) {
    return this.request({
      method: RequestMethod.Post,
      urlExtension: urlFn(options.urlParams),
      body: typeof options.body === 'string' ? options.body : JSON.stringify(options.body)
    })
  }

  public put (urlFn, options: IMethodOptions) {
    return this.request({
      method: RequestMethod.Put,
      urlExtension: urlFn(options.urlParams),
      body: typeof options.body === 'string' ? options.body : JSON.stringify(options.body)
    })
  }

  public remove (urlFn, options: IMethodOptions) {
    return this.request({
      method: RequestMethod.Delete,
      urlExtension: urlFn(options.urlParams)
    })
  }
}

```

## Using the REST service in a model

```javascript
import {RestClient} from './rest-client'

export interface IUserHobbyGet {
  urlParams: {
    hobbyId: string
  }
}

export interface IUserHobbyPost {
  urlParams: {
    hobbyId: string
  },
  body: {
    hobbyName: string,
    hoursSpentPerWeek: number,
    funOnAScale: number
  }
}

export class UserHobbyModel {
  constructor (private _restClient: RestClient) {}

  private _urlFn = o => `/user/${}/hobby/${o.hobbyId ? '/' + o.hobbyId : ''}`

  public get (options: IUserHobbyGet) {
    return this._restClient.get(this._urlFn, options)
  }

  public post (options: IUserHobbyPost) {
    return this._restClient.post(this._urlFn, options)
  }
}
```
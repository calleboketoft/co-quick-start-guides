# Angular 2 basic REST client

```javascript
import {Injectable} from 'angular2/http'
import {Request, Http, Headers} from 'angular2/http'

export const GET = 'GET'
export const POST = 'POST'
export const PUT = 'PUT'
export const DELETE = 'DELETE'

export interface RequestOptions {
  urlParams?: any;
  queryParams?: any;
  body?: any;
  method?: string;
}

@Injectable()
export class RestClient {
  constructor (private http: Http) {}

  private baseUrl = '';

  public getDefaultHeaders = () => ({'Content-Type': 'application/json'});

  public getFullUrl = (urlExt) => this.baseUrl + urlExt;

  public request (urlFn, {
    method,
    urlParams = {},
    queryParams = {},
    body
  }: RequestOptions) {
    body = (typeof body === 'string' ? body : JSON.stringify(body))
    let headers = new Headers(Ojbect.assign({}, this.getDefaultHeaders(), config.headers)
    return this.http.request(new Request({
      url: this.getFullUrl(urlFn(urlParams, queryParams)),
      method,
      headers,
      body
    }))
  }

  public get (urlFn, options = {}) {
    return this.request(urlFn, Object.assign(options, {method: GET})
  }

  public post (urlFn, options) {
    return this.request(urlFn, Object.assign(options, {method: POST}))
  }

  public put (urlFn, options) {
    return this.request(urlFn, Object.assign(options, {method: PUT}))
  }

  public remove (urlFn, options) {
    return this.request(urlFn, Object.assign(options, {method: DELETE}))
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

## Using the model

```javascript
this._userHobbyModel.get({urlParams: {userId: 'calle'}})
  .subscribe((res) => {
    console.log(res.json())
  })
```
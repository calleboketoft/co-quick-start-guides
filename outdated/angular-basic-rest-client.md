# Angular 2 basic REST client

```javascript
import { Injectable } from '@angular/core'
import { Request, Http, Headers } from '@angular/http'

export const GET = 'GET'
export const POST = 'POST'
export const PUT = 'PUT'
export const DELETE = 'DELETE'

export interface RequestOptions {
  pathParams?: any;
  queryParams?: any;
  body?: string;
  method?: string;
  headers?: any;
}

@Injectable()
export class RestService {
  constructor (private http: Http) {}

  private baseUrl = '';
  
  // Get query string from 1 level deep object of params
  public static getQueryStringFromObj (queryParams) {
    let urlSearchParams = new URLSearchParams()
    Object.keys(queryParams).forEach(key => {
      if (queryParams[key]) {
        urlSearchParams.set(key, queryParams[key])
      }
    })
    let queryString = urlSearchParams.toString()
    return queryString === '' ? queryString : '?' + queryString
  }

  public getDefaultHeaders = () => ({'Content-Type': 'application/json'});

  public getFullUrl = (urlExt) => this.baseUrl + urlExt;

  public request (urlFn, {
    method,
    pathParams = {},
    queryParams = {},
    body = '',
    headers
  }: RequestOptions) {
    let headersMerged = new Headers(Object.assign({}, this.getDefaultHeaders(), headers))
    return this.http.request(new Request({
      url: this.getFullUrl(urlFn(pathParams, queryParams)),
      method,
      headers: headersMerged,
      body
    }))
  }

  public get (urlFn, options = {}) {
    return this.request(urlFn, Object.assign(options, {method: GET}))
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
import { RestClient } from './rest-client'

export interface IUserHobbyGet {
  pathParams: {
    hobbyId: string
  },
  queryParams: {
    includeRelatives: boolean
    textFormat: string
  }
}

export interface IUserHobbyPost {
  pathParams: {
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

  private _urlFn = (pParams, qParams = {}) => {
    let queryString = this._restClient.getQueryStringFromObj(qParams)
    return`/user/${}/hobby/${pParams.hobbyId ? '/' + pParams.hobbyId : ''}${queryString}`
  }

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
this._userHobbyModel.get({
  urlParams: {
    userId: 'calle'
  },
  queryParams: {
    includeRelatives: true,
    textFormat: 'uppercase'
  }})
  .subscribe((res) => {
    console.log(res.json())
  })
```

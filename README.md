# adal-1015-angular5

Angular 5 Active Directory Authentication Library (ADAL) wrapper package. Can be used to authenticate Angular 5 applications to Azure Active Directory.

Based on https://github.com/grumar/adal-angular5.

This was updated to to use adal-angular 1.0.15 instead of 1.0.16 as 1.0.16 version of the library had issues with the aquire token function

## How to use it
> IMPORTANT!

Don't use `Http` and `HttpModule`, You definitely must use `HttpClient` and `HttpClientModule` instead of them.
The new interceptor is used only for request made by `HttpClient`.
When old `Http` used request will be untouched (no authorization header).

In `app.module.ts`

```typescript
import { HttpClient, HttpClientModule } from '@angular/common/http';
...
    imports: [..., HttpClientModule  ], // important! HttpClientModule replaces HttpModule
    providers: [
        Adal5Service,
        { provide: Adal5HTTPService, useFactory: Adal5HTTPService.factory, deps: [HttpClient, Adal5Service] } //  // important! HttpClient replaces Http
  ]
```

## Example

```typescript
import { Adal5HTTPService, Adal5Service } from 'adal-angular5';
...
export class HttpService {
    constructor(
        private adal5HttpService: Adal5HTTPService,
        private adal5Service: Adal5Service) { }

public get(url: string): Observable<any> {
        const options = this.prepareOptions();
        return this.adal5HttpService.get(url, options)
    }
    
private prepareOptions():any{
 let headers = new HttpHeaders();
        headers = headers
            .set('Content-Type', 'application/json')
            .set('Authorization', `Bearer ${this.adal5Service.userInfo.token}`);
        return { headers };
}
```        

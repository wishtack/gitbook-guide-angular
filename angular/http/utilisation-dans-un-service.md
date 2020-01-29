# Utilisation dans un Service

{% hint style="success" %}
Le service `HttpClient` ne devrait pas être utilisé directement depuis les composants.

Il faut "wrapper" l'interaction avec les APIs dans des **services dédiés** et **réutilisables**.
{% endhint %}

Il nous faudrait donc un service que l'on puisse utiliser ainsi depuis nos composants :

```typescript
bookRepository.getBookList()
    .subscribe(bookList => this.bookList = bookList);
```

## Transformation de la "response" avec un opérateur

Ce service retourne un `Observable`. La **transformation** des données doit donc se faire **avec un opérateur dans le service**.

{% tabs %}
{% tab title="book-repository.ts" %}
```typescript
@Injectable({
    providedIn: 'root'
})
export class BookRepository {

    private _bookListUrl = 'https://www.googleapis.com/books/v1/volumes?q=extreme%20programming';

    constructor(private _httpClient: HttpClient) {
    }

    getBookList() {
        return this._httpClient.get<GoogleVolumeListResponse>(this._bookListUrl)
            .pipe(map(googleVolumeListResponse => {

                const bookList = googleVolumeListResponse.items
                    .map(item => new Book({
                        title: item.volumeInfo.title
                    }));

                return bookList;

            }));
    }

}
```
{% endtab %}
{% endtabs %}

## Astuce : n'oubliez pas les metadata

Idéalement, pour faciliter l'extensibilité et la gestion de la pagination, pensez à produire un objet englobant la liste ainsi que les "metadata" associées _\(pagination etc...\)_.

```typescript
import { Observable, of } from 'rxjs';
import { map } from 'rxjs/operators';

interface ListResponse<T> {
    meta: {
        totalCount: number
    };
    itemList: T[];
}

class BookRepository {

    getBookList() {
        return this.getBookListWithMeta()
            .pipe(map(bookListResponse => bookListResponse.itemList));
    }

    getBookListWithMeta(): Observable<ListResponse<Book>> {
        return of({
            meta: {
                totalCount: 100
            },
            itemList: [
                new Book(),
                new Book()
            ]
        });
    }

}

new BookRepository().getBookListWithMeta()
    .subscribe(bookListResponse => {
        const bookCount = bookListResponse.meta.totalCount;
        const bookList = bookListResponse.itemList;
    });
```


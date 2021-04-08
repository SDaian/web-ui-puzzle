# T-Mobile Dev Puzzle

## Code Smells/Improvements
- Functions should have the returning type, if we are using the functions to just dispatch an action, use :void returning.
- formatDate function can be converted to a customPipe if cannot use the built-in pipe from Angular.
- When you use method .subscribe() you need to unsubscribe from that observable, maybe use the ngOnDestroy lifecycle, or takeUntil() method, in my opinion is better use async pipe on the template to delegate that unsubscription on the pipe.
- Can use a different changeDetectionStrategy, like OnPush, if your components are dumb, this will improve performance.
- Can adopt an pattern to avoid have the store instanciated in every component (for Ex. Facade Pattern).
- Separate the API calls on effects into a service file to improve readability of code
  - From this:

  `searchBooks$ = createEffect(() =>
    this.actions$.pipe(
      ofType(BooksActions.searchBooks),
      switchMap((action) =>
        this.http.get<Book[]>(`/api/books/search?q=${action.term}`).pipe(
          map((data) => BooksActions.searchBooksSuccess({ books: data })),
          catchError((error) => of(BooksActions.searchBooksFailure({ error })))
        )
      )
    )
  );`

  - To this:

  `searchBooks$ = createEffect(() =>
    this.actions$.pipe(
      ofType(BooksActions.searchBooks),
      switchMap((action) =>
        this.booksService.search(action.term).pipe(
          map((data) => BooksActions.searchBooksSuccess({ books: data })),
          catchError((error) => of(BooksActions.searchBooksFailure({ error })))
        )
      )
    )
  );`

  ## Accesibility
  ### Lighthouse report
  - Buttons don't have accesible names.
    - Button search and My Reading list button don't have accesible name, to fix this issue can add `aria-label`, this property was added on reading-list.component.
  - `<frame>` or `<iframe>` doesnt have title.
  - Background colours and foreground colours don't have a correct contrast ratio.
    - In the case of `Reading List` button, you can modify with this
      - font-weight: bold
      - font-size: 1.1667rem
    - In the case of `Try searching for a topic, for example "JavaScript".` you can modify with this:
      - color: $gray50 (according to https://www.farb-tabelle.de/en/rgb2hex.htm?q=gray50)
  
  ### Manual check
  - Button close on `My Reading List` section dont have accesible name.
  - Empty reading list in the same section before, dont have a correct constrast ratio.
  - Tag `<img>` dont have alt attribute
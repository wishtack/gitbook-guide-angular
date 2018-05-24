# Project Structure

Pour profiter au maximum du [Lazy Loading](lazy-loading.md), **pensez à bien décomposer vos applications en petits modules dès le départ** !

Il est également intéresser de séparer les [Routed Feature Modules](../project-structure-and-modules/feature-module.md) et les [Domain Feature Modules](../project-structure-and-modules/feature-module.md) ou les [Widget Feature Module](../project-structure-and-modules/feature-module.md)s tout en essayant de suivre avec les Routed Feature Modules l'arborescence de "Routing".

Les "Routed Feature Modules" ne contiennent que des **"View Components"**. **Ces composants suffixés par `View` sont couplés au "Routing"** _\(Cf._ [_https://blog.wishtack.com/2017/05/05/the-guide-to-building-quality-angular-2-components/_](https://blog.wishtack.com/2017/05/05/the-guide-to-building-quality-angular-2-components/)_\)_.

```bash
├── app-routing.module.ts
├── app.module.ts
├── auth
├── book
│   ├── book-list
│   ├── book-preview
│   └── book-search
├── user
└── views
    ├── admin
    │   ├── admin-routing.module.ts
    │   ├── dashboard-view
    │   └── settings
    │       ├── settings-routing.module.ts
    │       └── settings-view
    ├── book
    │   ├── book-detail-view
    │   ├── book-list-view
    │   └── book-routing.module.ts
    └── cart
        ├── cart-detail-view
        └── cart-routing.module.ts
```

**Les "Domain, Service et Widget Feature Modules"** _**\(`auth`, `book`, `user`\)**_ **sont indépendants du fonctionnement et de la configuration du "Routing"**. Ils peuvent être réutilisés peu importe le contexte.

En revanche les **Routed Modules** et les **View Components** sont fortement couplés au "Routing" et nous permettent d'imaginer rapidement la configuration du "Routing" :

```bash
/admin/dashboard => DashboardViewComponent
/admin/settings => SettingsRoutingModule => SettingsViewComponent
/book/detail/... => BookDetailView
/book/list => BookListView
/cart => CartDetailView
```




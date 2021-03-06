# Angular

---

## Run

```
$ git clone {url}
$ cd {project_dir}
$ npm install -g @angular/cli
$ npm install
$ ng serve
```

---

## 使用ライブラリ、フレームワーク

```

Package                           Version
------------------------------------------------
angular CLI                       7.1.4
node                              10.11.0
angular                           7.1.4
@angular-devkit/architect         0.11.4
@angular-devkit/build-angular     0.13.6
@angular-devkit/build-optimizer   0.13.6
@angular-devkit/build-webpack     0.13.6
@angular-devkit/core              7.1.4
@angular-devkit/schematics        7.1.4
@angular/cdk                      7.3.5
@angular/compiler                 7.2.10
@angular/compiler-cli             7.2.10
@angular/flex-layout              7.0.0-beta.24
@angular/material                 7.3.5
@ngtools/webpack                  7.3.6
@schematics/angular               7.1.4
@schematics/update                0.11.4
rxjs                              6.3.3
typescript                        3.1.6
webpack                           4.29.0
```

---

## 手順
### Angularをグローバルインストール

```
$ npm install -g @angular/cli
```

### プロジェクトを作成

```
$ ng new {project_name} --routing
$ cd {project_name}
```

###  Angular Materialをインストール

```
$ npm install --save @angular/material \
                     @angular/cdk \
                     @angular/animations
```

### Angular Materialをインポート

`./src/app/app.module.ts` に

```
import { BrowserAnimationsModule } from '@angular/platform-browser/animations';
import { MatButtonModule,
         MatCheckboxModule,
         MatSidenavModule,
         MatToolbarModule,
         MatIconModule,
         MatListModule,
         MatCardModule,
         MatTableModule } from '@angular/material';
```

BrowserAnimationModule,
[MatButtonModule](https://material.angular.io/components/button/overview)、[MatCheckboxModule](https://material.angular.io/components/checkbox/overview)をインポート。
ngModuleには

```
BrowserAnimationsModule,
MatButtonModule,
MatCheckboxModule,
MatSidenavModule,
MatToolbarModule,
MatIconModule,
MatListModule,
MatCardModule,
MatTableModule
```

を追加。

### SCSSをインポート

`./src/styles.scss` に
```
@import "~@angular/material/prebuilt-themes/indigo-pink.css";
```

を追加。

[prebuilt-themes](https://material.angular.io/guide/theming#defining-a-custom-theme)から選ぶことも、自分でテーマをビルドすることもできる。

### [Hammer.js](http://hammerjs.github.io/) をインストール

```
$ npm install --save hammerjs
```

### Hammer.jsをインポート

これは `./src/app/app.module.ts` ではなく、 `./src/main.ts` に

```
import 'hammerjs';
```

を追加。

### マテリアルアイコンを読み込み

`./src/index.html` にMaterial IconをCDNで読み込む。

```:CSS
<link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet">
```

### Angular Flexboxを追加

```
$ npm install @angular/flex-layout \
              rxjs-compat \
              @angular/compiler@7.2.10 \
              ajv@^6.9.1
```

### Angular Flexboxを読み込み
`./src/app/app.module.ts` に

```
import { FlexLayoutModule } from '@angular/flex-layout';
```

をインポート。NgModuleには

```
FlexLayoutModule
```

を追加。

### Normalize.cssをインストール

```
$ npm install normalize.css
```

### Normalize.cssを読み込み
`angular.json`の`styles`で`style.scss`を読み込んでいる部分の上に

```
"node_modules/normalize.css/normalize.css"
```
を追加。


### コンポーネントを作成

コンポーネント `HelloComponent` と `BoardComponent` をアプリケーションに定義する。

```
$ ng g c hello --module app.module
$ ng g c board --module app.module
```

なぜ `--module app.module` をつけるかというと、Angular CLIがターゲットのモジュールを自動的に識別できないようにする`material.module`という別のモジュールがあるため。

### Hello.component.html を編集

```
<div style="text-align:center">
  <h1>Hi!</h1>
  <p>
    This is the HelloComponent!
  </p>
</div>
```

### ルートを作成

複数のコンポーネントにアクセスできるように、ルートを定義する必要がある。

`src/app` に `app.routes.ts` を作成し、

```
import { NgModule} from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { HelloComponent } from './hello/hello.component';
import { BoardComponent } from './board/board.component';

const routes: Routes = [
  {path: '', component: HelloComponent},
  {path: 'board', component: BoardComponent}
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRouters {}
```

を追加する。

ここでは`HelloComponent`,`BoardComponent`を定義している。

これにより、`http://localhost:4200`にアクセスすると`HelloComponent`を表示し、`http://localhost:4200/board`にアクセスすると`BoardComponent`を表示する。

### `app.component.html` からコンポーネントへのリンクを追加する。

該当アイテムに対して `routerLink="/"`、`routerLink="/board"`を追加。

### AppRouterをインポート

`app.module.ts` に

```
import {AppRouters} from './app.routes';
```

を。importsに`AppRouters`を追加する。


### インスタンスを表すインターフェースを定義
`src/app/Post.ts` を作成し、

```
export interface Post {
  title: string;
  category: string;
  date_posted: Date;
  position: number;
  body: string;
}
```

を追加する。

これで、このインタフェースを使ってデータサービスを構築する。

### サービスを作成する

```
$ ng g s data/data --module app.module
```

を実行する。
作成した`data.service.ts`を

```
import {Injectable} from '@angular/core';
import {Post} from '../Post';
import {Observable, of} from 'rxjs';

@Injectable()
export class DataService {

  ELEMENT_DATA: Post[] = [
    {position: 0, title: 'Post One', category: 'Web Development', date_posted: new Date(), body: 'Body 1'},
    {position: 1, title: 'Post Two', category: 'Android Development', date_posted: new Date(), body: 'Body 2'},
    {position: 2, title: 'Post Three', category: 'IOS Development', date_posted: new Date(), body: 'Body 3'},
    {position: 3, title: 'Post Four', category: 'Android Development', date_posted: new Date(), body: 'Body 4'},
    {position: 4, title: 'Post Five', category: 'IOS Development', date_posted: new Date(), body: 'Body 5'},
    {position: 5, title: 'Post Six', category: 'Web Development', date_posted: new Date(), body: 'Body 6'},
  ];
  categories = [
    {value: 'Web-Development', viewValue: 'Web Development'},
    {value: 'Android-Development', viewValue: 'Android Development'},
    {value: 'IOS-Development', viewValue: 'IOS Development'}
  ];

  constructor() {
  }

  getData(): Observable<Post[]> {
    return of<Post[]>(this.ELEMENT_DATA);
  }

  getCategories() {
    return this.categories;
  }

  addPost(data) {
    this.ELEMENT_DATA.push(data);
  }

  deletePost(index) {
    this.ELEMENT_DATA = [...this.ELEMENT_DATA.slice(0, index), ...this.ELEMENT_DATA.slice(index + 1)];
  }

  dataLength() {
    return this.ELEMENT_DATA.length;
  }
}
```
に置き換える。

### データサービスの確認

データサービスには、2つの異なる配列がある。
それは、投稿のカテゴリを格納するための配列とブログの投稿を格納するための配列。

また、 `app.module.ts`ファイルの新しいProviderにDataServiceクラスを含まなければならない。
まずDataServiceをインポートし、
```
import {DataService} from './data/data.service';
```

ngModuleには
```
providers: [DataService],
```
を追加する。

これでデータサービスを利用してデータを表示、ボードを更新することができる。

`Board.component.ts`を次のコードに置き換える。

```
import {Component} from '@angular/core';
import {DataService} from '../data/data.service';
import {Post} from '../Post';
import {DataSource} from '@angular/cdk/table';
import {Observable} from 'rxjs/Observable';

@Component({
  selector: 'app-board',
  templateUrl: './board.component.html',
  styleUrls: ['./board.component.scss']
})
export class BoardComponent {
  constructor(private dataService: DataService) {
  }

  displayedColumns = ['date_posted', 'title', 'category', 'delete'];
  dataSource = new PostDataSource(this.dataService);
}

export class PostDataSource extends DataSource<any> {
  constructor(private dataService: DataService) {
    super();
  }

  connect(): Observable<Post[]> {
    return this.dataService.getData();
  }

  disconnect() {
  }
}
```



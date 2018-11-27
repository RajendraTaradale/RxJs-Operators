# RxJs-Operators

https://www.c-sharpcorner.com/blogs/operators

import { Component, OnInit } from '@angular/core';
import { Observable, of, from } from 'rxjs';
import { map, share, switchMap, tap} from 'rxjs/operators';
import { HttpClient } from '@angular/common/http';



@Component({
  selector: 'app-rxjs-operators',
  templateUrl: './rxjs-operators.component.html',
  styleUrls: ['./rxjs-operators.component.scss']
})
export class RxjsOperatorsComponent implements OnInit {
  loading: boolean;

  constructor(private http: HttpClient) { }

  ngOnInit() {
    const reqPosts = this.getPosts();
    const reqUsers = this.getUsers();

    const reqPostsUser = reqPosts.pipe(
      switchMap(posts => {
        return reqUsers.pipe(tap(users => {
          console.log('Posts List ', posts);
          console.log('User List ', users);
        }));
      })
    );

    reqPostsUser.subscribe();

    //Share() operator
    // this.setLoading(request);
    // request.subscribe(data => console.log(data));

    // of operator
    // const employee = {  
    //   name: 'Rajendra'  
    // };  
    // const obsEmployee: Observable<any>  = of(employee);  
    // const obsEmployee: Observable<any>  = of('Rajendra Taradale');
    // obsEmployee
    // .subscribe((data) => { console.log(data); });

    //Map Operators
    // const data = of('Rajendra Taradale');
    // data.pipe(map(x => x.toUpperCase()))
    // .subscribe((d) => { console.log(d); });

  }

  // setLoading(obs: Observable<any>) {
  //   this.loading = true;
  //   obs.subscribe(() => this.loading = false);
  // }

  getUsers(): Observable<any[]> {
    return this.http.get<any[]>('https://jsonplaceholder.typicode.com/users');
  }

  getPosts(): Observable<any[]> {
    return this.http.get<any[]>('http://jsonplaceholder.typicode.com/posts');
  }

}

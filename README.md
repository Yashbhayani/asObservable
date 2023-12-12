# In Angular, the asObservable method is often used in combination with RxJS to create observable streams from data or events. Assuming you want to retrieve data for a person's name and surname using observables, here's an example of how you might do it:

# Let's say you have a service (DataService) that fetches the person's data:

# Service File :-

import { Injectable } from '@angular/core'; <br/>
import { HttpClient } from '@angular/common/http';<br/>
import { Observable } from 'rxjs';<br/>
<br/>
@Injectable({
<br/>
  providedIn: 'root'
  <br/>
})
<br/>

export class DataService {
<br/>
  constructor(private http: HttpClient) {}
<br/>
  getPersonData(): Observable<any> {
  <br/>
    // Replace 'your-api-endpoint' with the actual endpoint to fetch the person's data
    <br/>
    return this.http.get('your-api-endpoint');
    <br/>
  }
  <br/>
}
<br/>
# Now, in a component, you can subscribe to this observable to get the person's name and surname:

# Ts File :-
<br/>
import { Component, OnInit } from '@angular/core';
<br/>
import { DataService } from 'path-to-your-data-service';
<br/>
import { map } from 'rxjs/operators';
<br/>
@Component({
<br/>
  selector: 'app-person',
  <br/>
  template: `
    <div *ngIf="personData$ | async as person">
      <br/>
      <p>Name: {{ person.name }}</p>
      <br/>
      <p>Surname: {{ person.surname }}</p>
      <br/>
    </div>
  `
  <br/>
})
<br/>
export class PersonComponent implements OnInit {
<br/>
  personData$: Observable<any>;
<br/>
  constructor(private dataService: DataService) {}
<br/>
  ngOnInit() {
    <br/>
    this.personData$ = this.dataService.getPersonData().pipe(
      <br/>
      map((data: any) => data.person) // Assuming the API returns data with a 'person' object containing name and surname
    <br/>
    );
    <br/>
  }
  <br/>
}
<br/>

# In this example, DataService fetches the person's data, returning an observable. The PersonComponent subscribes to this observable using the async pipe in the template. When the data arrives, it uses the map operator to extract the person object (assuming it's nested in the API response) and then displays the name and surname in the HTML template.

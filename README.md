# In Angular, the asObservable method is often used in combination with RxJS to create observable streams from data or events. Assuming you want to retrieve data for a person's name and surname using observables, here's an example of how you might do it:

# Let's say you have a service (DataService) that fetches the person's data:

# Service File :-

import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';

@Injectable({
  providedIn: 'root'
})
export class DataService {
  constructor(private http: HttpClient) {}

  getPersonData(): Observable<any> {
    // Replace 'your-api-endpoint' with the actual endpoint to fetch the person's data
    return this.http.get('your-api-endpoint');
  }
}

# Now, in a component, you can subscribe to this observable to get the person's name and surname:

# Ts File :-

import { Component, OnInit } from '@angular/core';
import { DataService } from 'path-to-your-data-service';
import { map } from 'rxjs/operators';

@Component({
  selector: 'app-person',
  template: `
    <div *ngIf="personData$ | async as person">
      <p>Name: {{ person.name }}</p>
      <p>Surname: {{ person.surname }}</p>
    </div>
  `
})
export class PersonComponent implements OnInit {
  personData$: Observable<any>;

  constructor(private dataService: DataService) {}

  ngOnInit() {
    this.personData$ = this.dataService.getPersonData().pipe(
      map((data: any) => data.person) // Assuming the API returns data with a 'person' object containing name and surname
    );
  }
}

# In this example, DataService fetches the person's data, returning an observable. The PersonComponent subscribes to this observable using the async pipe in the template. When the data arrives, it uses the map operator to extract the person object (assuming it's nested in the API response) and then displays the name and surname in the HTML template.

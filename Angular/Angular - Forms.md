** Angular Forms**

We can handle forms through angular and then choose if we want to submit something to the server with Angular's HTTP service. Angular ships with many powerful tools for handling forms.
```
// A sample form:
<form>
  <label>Name</label>
  <input type='text' name='name'>
  <label>Mail</label>
  <input type='text' name='email'>
  <button type='submit'>Save</button>
</form>
```
Angular allows us to retrieve the values from the form and check validations and all this happens with typescript. We need to be able to parse the values we enter and need an object to work with e.g
```
{
  value: {
    name: 'Mike',
    mail: 'mike@mike.com'
  }
  valid: true // metadata attached to the form
}
```
Angular makes it easy to retrieve the forms values and to see the state of the form.

*Template Driven vs Reactive approach*

There are two approaches to forms in Angular:

- Template-Driven: Angular infers the Form Object from the DOM.
- Reactive - Form is created programmatically and synchronized in the DOM.  

**Template Driven approach**

Angular forms don't have a post property to a url.
We import the Forms Module into our `imports` in the app.module file. This is included automatically when using the cli. Angular will automatically create a form when it detects a form element in html code. The form element serves as a selector. Angular will not automatically detect your inputs though. This is because you may not want to add all these elements as controls to your forms.
You need to register controls of your form manually. To do so, you simply add ngModel. ngModel can be used for 2 way databinding, but it's actually a bigger part of the Forms Module. You then add the name property aswell e.g
```
<input
    type="text"
    id="username"
    class="form-control"
    ngModel
    name="username">
```

In order to change the default behaviour of the form we use the `ngSubmit` directive:
`<form (ngSubmit)="onSubmit">`
This is added to the form html. When we submit the form the specified method is called in the component.

In order to see the form object we need to add a reference to it on the form element. We can then pass it as an argument to the method called in ngSubmit.
`<form (ngSubmit)="onSubmit(f)" #f="ngForm">`. Assigning the local reference #f to ngForm gives Angular access to the form it created automatically.

```
// HTML code:
<div class="container">
  <div class="row">
    <div class="col-xs-12 col-sm-10 col-md-8 col-sm-offset-1 col-md-offset-2">
      <form (ngSubmit)="onSubmit(f)" #f="ngForm"> // registers #f as a local reference and passes it into the on submit method
        <div id="user-data">
          <div class="form-group">
            <label for="username">Username</label>
            <input
              type="text"
              id="username"
              class="form-control"
              ngModel // adds the ngModel attribute
              name="username">
          </div>
          <button class="btn btn-default" type="button">Suggest an Username</button>
          <div class="form-group">
            <label for="email">Mail</label>
            <input
              type="email"
              id="email"
              class="form-control"
              ngModel
              name="email">
          </div>
        </div>
        <div class="form-group">
          <label for="secret">Secret Questions</label>
          <select
            id="secret"
            class="form-control"
            ngModel
            name="secret">
            <option value="pet">Your first Pet?</option>
            <option value="teacher">Your first teacher?</option>
          </select>
        </div>
        <button class="btn btn-primary" type="submit">Submit</button>
      </form>
    </div>
  </div>
</div>

\\ Component

import { Component } from '@angular/core';
import { NgForm } from "@angular/forms";

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {

  onSubmit(form: NgForm){ //gives us access to the form.
    console.log(form.value);
  }

  suggestUserName() {
    const suggestedName = 'Superuser';
  }
}

```

*Form State*

When we access the form object we can see a lot of different information about the forms state including things like:
- the registered form controls
- the property values
- dirty (has anything changed, will be true after the submit)
- disabled (would be true if the form was disabled for some reason)
- invalid (will change depending on whether any validation is added to the form)
- valid (opposite of the above)
- touched (did we click into any of the fields? Registers even if field is unchanged)

*Accessing the form with @ViewChild*

We could also access the form data using `@ViewChild`:
```
import { Component, ViewChild } from '@angular/core';
import { NgForm } from "@angular/forms";

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  @ViewChild('f') signUpForm: NgForm;

  // onSubmit(form: NgForm){
  //   console.log(form.value);
  // }

  onSubmit() {
    console.log(this.signUpForm);

  }

  suggestUserName() {
    const suggestedName = 'Superuser';
  }
}
```

This is useful if we need to access this form before it is submitted.

*Validation*

We can add validators to the template. As we are using the template driven approach we need to add these to the template. Angular has a required directive which overrides the standard html one.
```
<input
              type="email"
              id="email"
              class="form-control"
              ngModel
              name="email"
              required // directive which means it's required
              email> // directive which checks for an email
```

Angular adds it's own classes to the html when they are touched or valid or dirty.

Which Validators do ship with Angular?

Check out the Validators class: https://angular.io/docs/ts/latest/api/forms/index/Validators-class.html - these are all built-in validators, though that are the methods which actually get executed (and which you later can add when using the reactive approach).

For the template-driven approach, you need the directives. You can find out their names, by searching for "validator" in the official docs: https://angular.io/api?type=directive - everything marked with "D" is a directive and can be added to your template.

Additionally, you might also want to enable HTML5 validation (by default, Angular disables it). You can do so by adding the ngNativeValidate  to a control in your template.

```
// Example of form validation

<div class="form-group">
            <label for="email">Mail</label>
            <input
              type="email"
              id="email"
              class="form-control"
              ngModel
              name="email"
              required
              email
              #email="ngModel"> // exposes the email form-control object
              <span class="help-block" *ngIf="email.invalid && email.touched">Please enter a valid email</span> // A bootstrap class which is bound by ngIf as to wheteher the form is valid and has been touched.
          </div>

// We can also add CSS classes to touched and invalid elements:

input.ng-invalid.ng-touched {
  border: 1px solid red;
}
```

*Default values*
If we want to give default values for properties we can bind them to `[ngModel]`
```
<select
           id="secret"
           class="form-control"
           [ngModel]="defaultQuestion" // using one-way(property) binding we can apply default answers
           name="secret"
           required>
           <option value="pet">Your first Pet?</option>
           <option value="teacher">Your first teacher?</option>
         </select>
```

You can still use two-way binding to instantly output values
```
<div class="form-group">
          <textarea
            name="questionAnswer"
            rows="3"
            [(ngModel)]="answer"></textarea>
        </div>
```

*Grouping form controls*

If we have a large form we may want to group our form-controls. We may also want to validate specific form-areas individually.
We can do this with the `ngModelGroup` directive. It needs to be set to a string value.
```
<div id="user-data" ngModelGroup="userData">
  <div class="form-group">
    <label for="username">Username</label>
    <input
      type="text"
      id="username"
      class="form-control"
      ngModel
      name="username"
      required>
  </div>
  <button class="btn btn-default" type="button">Suggest an Username</button>
  <div class="form-group">
    <label for="email">Mail</label>
    <input
      type="email"
      id="email"
      class="form-control"
      ngModel
      name="email"
      required
      email
      #email="ngModel">
      <span class="help-block" *ngIf="email.invalid && email.touched">Please enter a valid email</span>
  </div>
</div>
```

This adds an extra field in values as well as in user controls. They gain controls for the whole group control and as such have access to valid, pristine etc.

```
<div id="user-data"
  ngModelGroup="userData" // this sets the form-control group
  #userData="ngModelGroup"> // this exposes the form group to Angular
```

*Radio Buttons*

Radio buttons work well using a `*ngFor loop`

```
// In the HTML:
<div class="radio" *ngFor="let gender of genders">
  <label>
    <input
      type="radio"
      name="gender"
      ng-model
      [value]="gender">
      {{ gender }}
  </label>
</div>

// In the component

genders = ['male', 'female'];
```

*Setting and patching form values*
We can patch values into the form using a variety of methods. The obvious one is two-way data binding, but you can use other methods.

```
// setValue() way (Unfortunately overrides all values but the username)
suggestUserName() {
    const suggestedName = 'Superuser';
    this.signUpForm.setValue({ // we can change it by accessing the form and using the setValue() method. This requires it be passed an object exactly in the same format as the object itself
      userData: {
        username: suggestedName,
        email: ''
      },
      secret: 'pet',
      questionAnswer: '',
      gender: 'male'
    });
  }
```

```
// Better approach: Accessing the form section of the signup form object. The signUpForm is essentially a container.
suggestUserName() {
    const suggestedName = 'Superuser';
    this.signUpForm.form.patchValue({ // we can us =e the patchValue() method to change specific values
      userData: {
        username: suggestedName
      }
    })
  }
}
```

*Using Form Data*

We can create an empty object in our component and assign the form values to that object on submit. e.g
```
user = {
    username: '',
    email: '',
    secretQuestion: '',
    secretAnswer: '',
    gender: ''
  };
  submitted = false;

  onSubmit() {
    this.submitted = true;
    this.user.username = this.signUpForm.value.userData.username;
    this.user.email = this.signUpForm.value.userData.email;
    this.user.secretQuestion = this.signUpForm.value.secret;
    this.user.secretAnswer = this.signUpForm.value.questionAnswer;
    this.user.gender = this.signUpForm.value.gender;
  }
```

*Resetting the form*
We can quite easily reset the form by calling `this.signUpForm.reset();`. This can be passed the same object that we pass to the value method in order to reset only specific values.

**Reactive Forms**

With a reactive form, the form is created programmatically and synchronized with the DOM. We need to import the `ReactiveFormsModule` into the app.module instead of the regular `FormsModule`. We also need to create a form with the property of FormGroup within our component.
```
// In the app.module
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { ReactiveFormsModule } from '@angular/forms';
import { HttpModule } from '@angular/http';

import { AppComponent } from './app.component';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    ReactiveFormsModule,
    HttpModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }

// In the component:

signUpForm: FormGroup;
```

It is good practice to initialize a form during the OnInit lifecycle hook. This can be done as so:
```
ngOnInit() {
    this.signUpForm = new FormGroup({
      'username': new FormControl(null), // wrapped in quotation marks to make sure this property is usable after minification
      'email': new FormControl(null),
      'gender': new FormControl('male')
    });
  }
```
`new FormGroup({})` takes an object as an argument. The object contains each of the FormControls we want to associate with the form. We initialize new FormControls with `new FormControl(null)`. The first argument the constructor takes is the default value of the form control although it can take more arguments.

*Synching HTML and Form*

In order to sync the form with the HTML we need to add some directives to the form.
`<form [formGroup]="signUpForm">` overrides Angular's standard behaviour and tells it that the form group is the form that we have created in the component.
We also add the FormControls to the input options:
```
<div class="container">
  <div class="row">
    <div class="col-xs-12 col-sm-10 col-md-8 col-sm-offset-1 col-md-offset-2">
      <form [formGroup]="signUpForm">
        <div class="form-group">
          <label for="username">Username</label>
          <input
            type="text"
            id="username"
            formControlName="username"
            class="form-control">
        </div>
        <div class="form-group">
          <label for="email">email</label>
          <input
            type="text"
            id="email"
            formControlName="email"
            class="form-control">
        </div>
        <div class="radio" *ngFor="let gender of genders">
          <label>
            <input
              type="radio"
              formControlName="gender"
              [value]="gender">{{ gender }}
          </label>
        </div>
        <button class="btn btn-primary" type="submit">Submit</button>
      </form>
    </div>
  </div>
</div>
```

*Submitting the form*

`(ngSubmit)="onSubmit()` can be called within the form tag. We don't need to pass local references as the form is defined within the component itself.

*Validation*

To add validation to our form it needs to be added in the FormControl constructor.
```
this.signUpForm = new FormGroup({
  'username': new FormControl(null, Validators.required), // we don't execute this method. We reference it and Angular will execute it
  'email': new FormControl(null, [Validators.required, Validators.email]), // we can add multiple Validators in an array
  'gender': new FormControl('male')
});
```

*Getting access to the form controls*
We can use the form status to display messages but we need to access the form controls differently.

```
 <span class="help-block" *ngIf="signUpForm.get('username').invalid && signUpForm.get('username').touched">Please enter a valid username</span>
 ```

 We access the FormControl's state using the get method.

 *Form Groups*

 We can create FormGroups using the FormGroup constructor. It can be passed into the main FormGroup constructor.
 ```
 // In the component

 ngOnInit() {
    this.signUpForm = new FormGroup({
      'userData': new FormGroup({
        'username': new FormControl(null, Validators.required),
        'email': new FormControl(null, [Validators.required, Validators.email]),
      }),
      'gender': new FormControl('male')
    });
  }

 // In the HTML

 <div formGroupName="userData"> // we add this directive to a div that holds the formcontrols in the formgroups.
          <div class="form-group">
            <label for="username">Username</label>
            <input
              type="text"
              id="username"
              formControlName="username"
              class="form-control">
              <span class="help-block" *ngIf="signUpForm.get('userData.username').invalid && signUpForm.get('userData.username').touched">Please enter a valid username</span> // we need to add a relative path to the get method
          </div>
          <div class="form-group">
            <label for="email">email</label>
            <input
              type="text"
              id="email"
              formControlName="email"
              class="form-control">
              <span class="help-block" *ngIf="signUpForm.get('userData.email').invalid && signUpForm.get('userData.email').touched">Please enter a valid email</span>
          </div>
        </div>
 ```

*Arrays of FormControls*

We can dynamically add form controls using FormArray. A FormArray holds an array of FormControls which can be iterated through in order to display new form controls. It is good if we want to add new FormControls dynamically.
```
// In the component:

getControls(): AbstractControl[] {
    return (<FormArray>this.signUpForm.get('hobbies')).controls;
}

ngOnInit() {
   this.signUpForm = new FormGroup({
     'userData': new FormGroup({
       'username': new FormControl(null, Validators.required),
       'email': new FormControl(null, [Validators.required, Validators.email]),
     }),
     'gender': new FormControl('male'),
     'hobbies': new FormArray([]) // A form array holds an array of controls
   });
 }

 onSubmit(){
   console.log(this.signUpForm);

 }

 onAddHobby(){
   const control = new FormControl(null, Validators.required);
   (<FormArray>this.signUpForm.get('hobbies')).push(control); // we have to explicitly tell typescript that this is a FormArray be casting it
 }

 // In the HTML

 <div formArrayName="hobbies">
  <h4>Your Hobbies</h4>
  <button
    class="btn btn-default"
    type='button'
    (click)="onAddHobby()">Add Hobby</button>
    <div
      class="form-group"
      *ngFor="let hobbyControl of getControls(); let i = index">
      <input type="text" class="form-control" [formControlName]="i">
    </div> // we have to generate a formcontrol name using i
</div>
```

*Creating Custom Validators*

We can cover most of our use cases with the built in validators, but just in case, we are able to create our own custom validators.
A validator is essentially a method and we can easily create our own:
```
ngOnInit() {
    this.signUpForm = new FormGroup({
      'userData': new FormGroup({
        'username': new FormControl(null, [Validators.required, this.forbiddenNames.bind(this)]), // we need to bind this for our function to work properly
        'email': new FormControl(null, [Validators.required, Validators.email]),
      }),
      'gender': new FormControl('male'),
      'hobbies': new FormArray([]) // A form array holds an array of controls
    });
  }

forbiddenNames(control: FormControl): {[s: string]: boolean} {
  if (this.forbiddenUsernames.indexOf(control.value) !== -1) { // we need to check that it is not equal to -1. This is because if it doesn't exist in the array, it's index is -1 which evaluates to true
    return {'nameIsForbidden': true}
  }
  return null;
}
```

*Using Error Codes*
Angular adds the error codes to the individual controls on the errors object.
```
<span class="help-block" *ngIf="signUpForm.get('userData.username').invalid && signUpForm.get('userData.username').touched">
  <span class="help-block" *ngIf="signUpForm.get('userData.username').errors['nameIsForbidden']">Name is invalid</span>
  <span class="help-block" *ngIf="signUpForm.get('userData.username').errors['required']">Name is required</span>
</span>
```
You can display messages based on the errors you receive.

*Creating custom Async Validators*

Sometimes we may need to reach out to a web server to see if a username is taken or not. We can create such async validators.

We add async validators as a third element that is passed to form control:
```
'email': new FormControl(null // initial value, [Validators.required, Validators.email] // regular validators, this.forbiddenEmails // async validators),

forbiddenEmails(control: FormControl): Promise<any> | Observable<any> {
   const promise = new Promise<any>((resolve, reject) => {
     setTimeout(() => {
       if (control.value === 'test@test.com') {
         resolve({'emailIsForbidden': true });
       } else {
         resolve(null);
       }
     }, 1500)
   })
   return promise;
 }
```

*Reacting to status or value changes*
We saw that when using asynchronous validators, the form control changes from invalid to pending to valid. We can detect formstate with an observable.  `this.signupForm.statusChanges` and `this.signupForm.valueChanges`

```
ngOnInit() {
   this.signUpForm = new FormGroup({
     'userData': new FormGroup({
       'username': new FormControl(null, [Validators.required, this.forbiddenNames.bind(this)]),
       'email': new FormControl(null, [Validators.required, Validators.email], this.forbiddenEmails),
     }),
     'gender': new FormControl('male'),
     'hobbies': new FormArray([]) // A form array holds an array of controls
   });
   // this.signUpForm.valueChanges
   //   .subscribe(
   //     (value) => console.log(value)
   //   )
   this.signUpForm.statusChanges
     .subscribe(
       (value) => console.log(value)
     )
 }
```
You can also subscribe to specific form controls using get.

*Setting and patching values*
`setValue()` and `patchValue()` methods are also available. We need to pass in an object that resembles the form object. We can also pass an object to reset.

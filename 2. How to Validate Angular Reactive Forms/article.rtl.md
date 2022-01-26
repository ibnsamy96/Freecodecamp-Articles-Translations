# كيف تتحقق (Validate) من النماذج التفاعلية (Reactive Forms) في إطار عمل Angular

![How to Validate Angular Reactive Forms](https://cdn-media-2.freecodecamp.org/w1280/5f9c9e95740569d1a4ca3de6.jpg)

## مقدمة

في هذا المقال، سنتعرف على عمليات التحقق (Validations) من صحة النماذج التفاعلية في إطار عمل Angular. سننشئ نموذج تسجيل بسيط لمستخدم جديد وننفذ بعض عمليات التحقق (Validations) باستخدام المُدقّقات (Validators) المُدمجة في Angular. و بالإضافة إلى ذلك، سنقوم ببناء بعض المُدقّقات (Validators) الخاصة بنا لتنفيذ بعض عمليات التحقق للنموذج التفاعلي (Reactive Form).

سنعمل على التحقق من النقاط التالية خلال استعراضنا للـ Validations في هذا المقال:

- سنتحقق من توفر اسم المستخدم
- سنتحقق من توافق كلمة المرور مع نمط (Pattern) معيّن
- سنتحقق من تطابق كلمة المرور المدخلة في حقلين مختلفين (Two Fields)

ألق نظرة على التطبيق الذي سنبنيه في هذا المقال.

![](https://www.freecodecamp.org/news/content/images/2019/12/reactiveFormValidation.gif)

## الأدوات الأساسية التي تحتاجها

- قم بتثبيت Visual Studio Code من [هنا](https://code.visualstudio.com/)
- قم بتثبيت أحدث إصدار من Angular CLI من [هنا](https://cli.angular.io/)

## الكود المصدري

احصل على شفرة المصدر من [مستودع أكواد كاتب المقال على GitHub](https://github.com/AnkitSharma-007/angular-forms-validation)

## إنشاء تطبيق Angular

انتقل إلى المجلد حيث تريد إنشاء ملف المشروع الخاص بك. افتح نافذة الأوامر (Commands Window) وقم بتنفيذ الأمر التالي:

```bash
ng new angular-forms-validation --routing=false --style=scss
```

بكتابتنا لهذا الـ Command نحدد أننا نرغب بإنشاء تطبيق Angular جديد باسم `angular-Forms-validation` و بدون إضافة Routing. كذلك حددنا أننا سنضيف الـ Styles باستخدام ملفات "scss".

انتقل إلى مجلّد المشروع الجديد وافتحه في VS Code باستخدام الـ Commands الموضحة:

```bash
cd angular-forms-validation
code .
```

## تثبيت Bootstrap

قم بكتابة الأمر التالي لتثبيت مكتبة Bootstrap:

```bash
npm install bootstrap --save
```

و أضف سطر الاستيراد التالي في ملف `styles.scss`:

```css
@import "~bootstrap/dist/css/bootstrap.css";
```

## إنشاء Service للتحقق من صحة بيانات الـ Form

قم بكتابة الأمر التالي لإنشاء Service جديدة:

```bash
ng g s services\customvalidation
```

سينشئ هذا الأمر مجلدًا باسم 'services' يحتوي على ملف "customvalidation.service.ts" و ملف "customvalidation.service.spec.ts".  
افتح ملف "customvalidation.service.ts" وضع الكود التالي به:

```js
import { Injectable } from "@angular/core";
import { ValidatorFn, AbstractControl } from "@angular/forms";
import { FormGroup } from "@angular/forms";

@Injectable({
  providedIn: "root",
})
export class CustomvalidationService {
  patternValidator(): ValidatorFn {
    return (control: AbstractControl): { [key: string]: any } => {
      if (!control.value) {
        return null;
      }
      const regex = new RegExp("^(?=.*?[A-Z])(?=.*?[a-z])(?=.*?[0-9]).{8,}$");
      const valid = regex.test(control.value);
      return valid ? null : { invalidPassword: true };
    };
  }

  MatchPassword(password: string, confirmPassword: string) {
    return (formGroup: FormGroup) => {
      const passwordControl = formGroup.controls[password];
      const confirmPasswordControl = formGroup.controls[confirmPassword];

      if (!passwordControl || !confirmPasswordControl) {
        return null;
      }

      if (
        confirmPasswordControl.errors &&
        !confirmPasswordControl.errors.passwordMismatch
      ) {
        return null;
      }

      if (passwordControl.value !== confirmPasswordControl.value) {
        confirmPasswordControl.setErrors({ passwordMismatch: true });
      } else {
        confirmPasswordControl.setErrors(null);
      }
    };
  }

  userNameValidator(userControl: AbstractControl) {
    return new Promise((resolve) => {
      setTimeout(() => {
        if (this.validateUserName(userControl.value)) {
          resolve({ userNameNotAvailable: true });
        } else {
          resolve(null);
        }
      }, 1000);
    });
  }

  validateUserName(userName: string) {
    const UserList = ["ankit", "admin", "user", "superuser"];
    return UserList.indexOf(userName) > -1;
  }
}
```

يتم استخدام دالة `patternValidator` للتحقق من صحة نمط (Patterm) كلمات المرور في النموذج الخاص بنا. الـ Parameter لهذه الـ Method من نوع `AbstractControl` والذي يعتبر الـ Class الأساسية للـ `FormControl`.

سنستخدم التعبير النَمَطِي (Regular Expression) للتحقق من صحة كلمة المرور حيث سنتحقق من الشروط الأربعة التالية:

- يجب ألا تقل أحرف كلمة المرور عن ثمانية أحرف.
- يجب أن تحتوي على حرف صغير واحد على الأقل.
- يجب أن تحتوي على حرف كبير واحد على الأقل.
- يجب أن تحتوى على رقم واحد على الأقل.

إذا فشلت كلمة المرور في فحص regex ، فسنقوم بتغيير قيمة `invalidPassword` إلى "true".

تُستخدم دالة `MatchPassword` لمقارنة كلمات المرور في حقلين. ستقبل هذه الدالة Two Parameters من نوع string. تمثل هذه الـ parameters قيم الـ name attributes للحقول المطلوب مطابقتها. و عن طريق الـ `FormControl` لهذين الحقلين نطابق القيم الموجودة فيهما. إذا كانت القيم غير متطابقة ، فسنقوم بتغيير قيمة `passwordMismatch` إلى "true".

بينما يتم استخدام `userNameValidator` للتحقق مما إذا كان اسم المستخدم مستخدمًا بالفعل. ستقبل هذه الدالة parameter من النوع `AbstractControl`. سوف نتحقق مما إذا كانت قيمة هذا الحقل موجودة في مصفوفة `UserList`. إذا كانت القيمة التي أدخلها المستخدم موجودة بالفعل ، فسنقوم بتغيير قيمة `userNameNotAvailable` إلى "true".

و تُستخدم `setTimeout` لعمل استدعاء (Invoking) لهذه العملية كل ثانيتين مما سيضمن ظهور الخطأ بعد ثانيتين من وقت توقف المستخدم عن الكتابة في الحقل.

> من أجل تبسيط هذه المقالة ، نستخدم مصفوفة للبحث عن توفر أسماء المستخدمين بينما في الحالة الطبيعية يتم إرسال طلب (Request) للخادم (Server) للبحث عن اسم المستخدم في قاعدة البيانات (Database).

## إنشاء مكوّن النموذج التفاعلي (The Reactive Form Component)

قم بكتابة الأمر التالي لإنشاء مكوّن النموذج التفاعلي:

```bash
ng g c reactive-form
```

افتح ملف `reactive-form.component.ts` و ضع الكود التالي به:

```js
import { Component, OnInit } from '@angular/core';
import { Validators, FormGroup, FormBuilder } from '@angular/forms';
import { CustomvalidationService } from '../services/customvalidation.service';

@Component({
  selector: 'app-reactive-form',
  templateUrl: './reactive-form.component.html',
  styleUrls: ['./reactive-form.component.scss']
})
export class ReactiveFormComponent implements OnInit {

  registerForm: FormGroup;
  submitted = false;

  constructor(
    private fb: FormBuilder,
    private customValidator: CustomvalidationService
  ) { }

  ngOnInit() {
    this.registerForm = this.fb.group({
      name: ['', Validators.required],
      email: ['', [Validators.required, Validators.email]],
      username: ['', [Validators.required], this.customValidator.userNameValidator.bind(this.customValidator)],
      password: ['', Validators.compose([Validators.required, this.customValidator.patternValidator()])],
      confirmPassword: ['', [Validators.required]],
    },
      {
        validator: this.customValidator.MatchPassword('password', 'confirmPassword'),
      }
    );
  }

  get registerFormControl() {
    return this.registerForm.controls;
  }

  onSubmit() {
    this.submitted = true;
    if (this.registerForm.valid) {
      alert('Form Submitted succesfully!!!\n Check the values in browser console.');
      console.table(this.registerForm.value);
    }
  }
}
```

سنقوم بإنشاء متغير `registerForm` من النوع `FormGroup` و في دالة `ngOnInit` سنقوم بتحديد عناصر تحكُّم النموذج (Form Controls) باستخدام Class معينة و هي `FormBuilder`. تم تحديد كافة حقول النموذج كحقول إجبارية (Required). سنقوم بعمل Invoking لدالة `userNameValidator` الموجودة في الـ '`customValidator` Service' مع استخدام دالة الربط (Bind).

بالنسبة لحقل كلمة المرور ، سنستخدم دالة الإنشاء (Compose) لدمج عدة Validators في دالة واحدة. سنقوم أيضًا بعمل Invoking لدالة `MatchPassword` و تمرير قيم الـ name attributes لعناصر تحكم النموذج `password` و `ConfirmPassword` كـ parameters.

أما دالة `registerFormControl` فهي ستُرجِع (Return) عناصر تحكم النموذج. و دالة "onSubmit" ستطبع محتوى النموذج في الـ Console إذا كان النموذج صالحًا وتم إرساله بنجاح.

افتح ملف `reactive-form.component.html` و اكتب الكود التالي به:

```html
<div class="container">
  <div class="row">
    <div class="col-md-8 mx-auto">
      <div class="card">
        <div class="card-header">
          <h3>Angular Reactive Form</h3>
        </div>
        <div class="card-body">
          <form class="form" [formGroup]="registerForm" (ngSubmit)="onSubmit()">
            <div class="form-group">
              <label>Name</label>
              <input type="text" class="form-control" formControlName="name" />
              <span
                class="text-danger"
                *ngIf="(registerFormControl.name.touched || submitted) && registerFormControl.name.errors?.required"
              >
                Name is required
              </span>
            </div>
            <div class="form-group">
              <label>Email</label>
              <input type="text" class="form-control" formControlName="email" />
              <span
                class="text-danger"
                *ngIf="(registerFormControl.email.touched || submitted) && registerFormControl.email.errors?.required"
              >
                Email is required
              </span>
              <span
                class="text-danger"
                *ngIf="registerFormControl.email.touched && registerFormControl.email.errors?.email"
              >
                Enter a valid email address
              </span>
            </div>
            <div class="form-group">
              <label>User Name</label>
              <input
                type="text"
                class="form-control"
                formControlName="username"
              />
              <span
                class="text-danger"
                *ngIf="(registerFormControl.username.touched || submitted) && registerFormControl.username.errors?.required"
              >
                User Name is required
              </span>
              <span
                class="text-danger"
                *ngIf="registerFormControl.username.touched && registerFormControl.username.errors?.userNameNotAvailable"
              >
                User Name is not available
              </span>
            </div>
            <div class="form-group">
              <label>Password</label>
              <input
                type="password"
                class="form-control"
                formControlName="password"
              />
              <span
                class="text-danger"
                *ngIf="(registerFormControl.password.touched || submitted) && registerFormControl.password.errors?.required"
              >
                Password is required
              </span>
              <span
                class="text-danger"
                *ngIf="registerFormControl.password.touched && registerFormControl.password.errors?.invalidPassword"
              >
                Password should have minimum 8 characters, at least 1 uppercase
                letter, 1 lowercase letter and 1 number
              </span>
            </div>
            <div class="form-group">
              <label>Confirm Password</label>
              <input
                type="password"
                class="form-control"
                formControlName="confirmPassword"
              />
              <span
                class="text-danger"
                *ngIf="(registerFormControl.confirmPassword.touched || submitted)&& registerFormControl.confirmPassword.errors?.required"
              >
                Confirm Password is required
              </span>
              <span
                class="text-danger"
                *ngIf="registerFormControl.confirmPassword.touched && registerFormControl.confirmPassword.errors?.passwordMismatch"
              >
                Passwords doesnot match
              </span>
            </div>
            <div class="form-group">
              <button type="submit" class="btn btn-success">Register</button>
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>
</div>
```

سننشئ نموذجًا تفاعليًا (Reactive Form) ونستخدم Bootstrap لإضافة شكل و تصميم باستخدام الـ Cards. سيحتوي رأس البطاقة (Card Header) على عنوان بينما سيحتوي الجزء المركزي منها (Card Body) على حقول النموذج. سنقوم بربط خاصية `formGroup` لـ Tag النموذج `<form>` باسم النموذج الخاص بنا وهو` registerForm`.  
سيتم استدعاء دالة "onSubmit" عند إرسال النموذج.
سنقوم أيضًا بربط `formControlName` لكل حقل إدخال باسم عنصر التحكم الخاص بـ `FormGroup`. سوف نتحقق من وجود أخطاء في عناصر التحكم في النموذج ثم نعرض رسالة الخطأ المناسبة على الشاشة.

## إنشاء مكوّن شريط التنقل (The Nav-Bar Component)

لإنشاء مكوّن شريط التنقل قم بكتابة الأمر التالي:

```bash
ng g c nav-bar
```

افتح ملف `nav-bar.component.html` و اكتب الكود التالي به:

```html
<nav class="navbar navbar-expand-sm navbar-dark bg-dark fixed-top">
  <a class="navbar-brand" [routerLink]='["/"]'>Form Validation Demo</a>
  <div class="collapse navbar-collapse">
    <ul class="navbar-nav mr-auto">
      <li class="nav-item">
        <a class="nav-link" [routerLink]='["/reactive-form"]'>Reactive Form</a>
      </li>
    </ul>
  </div>
</nav>
```

و بهذا نكون وضعنا رابط الانتقال لـ مكوّن النموذج التفاعلي (Reactive Form Component) في شريط التنقّل (Nav-Bar).

## تعديل المكوّن المركزي في التطبيق (The App Component)

افتح ملف `app.component.html` و اكتب الكود التالي به:

```html
<app-nav-bar></app-nav-bar>
<div class="container">
  <router-outlet></router-outlet>
</div>
```

## تعديل وحدة التحكم المركزية في التطبيق (The App Module)

في ملف وحدة التحكم المركزية سنقوم بعمل Importing لـ Forms Module و تعريف قواعد الـ Routing المناسبة للتطبيق. و يمكنك الرجوع إلى [GitHub](https://github.com/AnkitSharma-007/angular-forms-validation/blob/master/src/app/app.module.ts) للوصول للكود المصدري الكامل لهذا الملف من مستودع الأكواد الخاص بكاتب المقال الأصلي.

أضف الكود التالي لملف `app.module.ts`:

```js
import { RouterModule } from '@angular/router';
import { ReactiveFormsModule } from  '@angular/forms';

@NgModule({
  ...
  imports: [
    ...
    ReactiveFormsModule,
    RouterModule.forRoot([
      { path: '', component: ReactiveFormComponent },
      { path: 'reactive-form', component: ReactiveFormComponent }
    ]),
  ],
})
```

## وقت تجربة التطبيق

سنستخدم الـ Command التالي كي يبدأ الـ Web Server الخاص بنا و نجرب التطبيق:

```bash
ng serve -o
```

ما سيفعله هذا التطبيق أنه سيفتح الرابط `http://localhost:4200` في نافذة جديدة على متصفحك الأساسي و يمكنك حينها أن تتحقق من حقول النموذج لتتأكد من ملاءمتها للشكل الذي برمجناها عليه.

يمكنك الانتقال إلي [https://ng-forms-validation.herokuapp.com](https://ng-forms-validation.herokuapp.com) إذا ما أردت تجربة نسخة حيّة (live) من هذا التطبيق بنفسك.

## الخلاصة

قمنا بعمل نموذج تسجيل مستخدم جديد في Angular باستخدام طريقة النماذج التفاعلية (Reactive Forms) و زوّدناها ببعض الـ Validaions التي بنيناها بأنفسنا بالإضافةلاستخدام بعض الـ Validadors الموجودة بالفعل في Angular. و تم استخدام Bootstrap لإضافة Styling للتطبيق.

يمكنك الحصول على [الكود المصدري الكامل للتطبيق من مستودع كاتب المقال الأصلي على GitHub](https://github.com/AnkitSharma-007/angular-forms-validation) و يمكنك التعديل و الإضافة عليه كي يساعدك ذلك على فهم أفضل.

## مقالات مشابهة بقلم الكاتب

- [Localization In Angular Using i18n Tools](https://ankitsharmablogs.com/localization-in-angular-using-i18n-tools/)
- [Template-Driven Form Validation In Angular](https://ankitsharmablogs.com/template-driven-form-validation-in-angular/)
- [Understanding Angular Animation](https://ankitsharmablogs.com/understanding-angular-animation/)
- [Policy-Based Authorization In Angular Using JWT](https://ankitsharmablogs.com/policy-based-authorization-in-angular-using-jwt/)
- [Facebook Authentication And Authorization In Server-Side Blazor App](https://ankitsharmablogs.com/facebook-authentication-and-authorization-in-server-side-blazor-app/)

و يمكنك قراءة مقالات مشابهة لهذا المقال من خلال [مدونة كاتب المقال الأصل Ankit Sharma](https://ankitsharmablogs.com/).

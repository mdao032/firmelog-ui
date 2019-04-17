##login.component.html : 

<div class="login-page" [@routerTransition] [ngClass]="globals.theme">
    <div class="row justify-content-md-center">
        <div class="col-md-4">
            <img src="../../assets/images/logo-150x150.png" width="150px"/>
            <h1>test</h1>
            <form role="form">
                <div class="form-content">
                    <div class="form-group">
                        <input type="text" [(ngModel)]="login" class="form-control input-underline input-lg"
                               placeholder="{{ 'Sesame user name' | translate }}" name="login">
                    </div>

                    <div class="form-group">
                        <input [type]="showPassword? 'text': 'password'" [(ngModel)]="password"
                               class="form-control input-underline input-lg"
                               placeholder="{{ 'Sesame password' | translate }}" name="password">
                        <i class="fa pull-right" [ngClass]="showPassword? 'fa-eye-slash': 'fa-eye'" (click)="showPasswordToggle()"></i>
                    </div>
                </div>
                <button type="submit" [disabled]="!login || !password || signingIn" class="btn rounded-btn"
                        (click)="onLoggedIn()">
                    {{ 'Log in' | translate }}
                    <i class="fa fa-spinner fa-spin" *ngIf="signingIn"></i>
                </button>
            </form>
        </div>
    </div>
</div>


##Login.component.ts

import {Component, OnInit} from '@angular/core';
import {Router} from '@angular/router';
import {TranslateService} from '@ngx-translate/core';
import {routerTransition} from '../router.animations';
import {Globals} from '../shared/Globals';
import {Utils} from '../shared/Utils';

@Component({
    selector: 'app-login',
    templateUrl: './login.component.html',
    styleUrls: ['./login.component.scss', './login-blue.component.scss', './login-dark.component.scss'],
    animations: [routerTransition()]
})
export class LoginComponent implements OnInit {
    public login: string;
    public password: string;
    public language: string;
    public showPassword: boolean;
    public signingIn: boolean;

    constructor(private translate: TranslateService, public router: Router, public globals: Globals) {
        this.translate.addLangs(['en', 'fr']);
        this.translate.setDefaultLang('fr');
        this.language = Utils.getLanguage(this.translate.getBrowserLang());
        this.translate.use(this.language);
        this.signingIn = false;
    }

    ngOnInit() {
        this.showPassword = false;
    }

    showPasswordToggle(){
        this.showPassword = !this.showPassword;
    }

    onLoggedIn() {
        this.signingIn = true;
        localStorage.setItem('isLoggedin', 'true');
        this.router.navigate(['dashboard']);
    }
}


##Login.module.ts

import {NgModule} from '@angular/core';
import {CommonModule} from '@angular/common';
import {TranslateModule} from '@ngx-translate/core';

import {LoginRoutingModule} from './login-routing.module';
import {LoginComponent} from './login.component';
import {FormsModule} from '@angular/forms';

@NgModule({
    imports: [
        CommonModule,
        TranslateModule,
        LoginRoutingModule,
        FormsModule],
    declarations: [LoginComponent]
})
export class LoginModule {

}


##Login-routing.module.ts

import { NgModule } from '@angular/core';
import { Routes, RouterModule } from '@angular/router';
import { LoginComponent } from './login.component';

const routes: Routes = [
    {
        path: '',
        component: LoginComponent
    }
];

@NgModule({
    imports: [RouterModule.forChild(routes)],
    exports: [RouterModule]
})
export class LoginRoutingModule {}

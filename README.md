![Captura de Tela (147)](https://github.com/PauloAquarius0299/desafio-frontend-smartfit/assets/114706743/c4cd2042-4ae1-470f-add5-a7ef90b57c98)

# 🏋️ Desafio Front-End SmartFit 
Usei o desafio da SmartFit para aprimorar minhas habilidades em Angular com TypeScript
## 🏋️‍♀️ Introdução 
O desafio era montar uma interface que filtrasse as 167 lojas da SmartFit e suas funcionalidades, durante a pandemia, com a http JSON que a empresa disponibilizou no link do desafio.
### 💡 Pre-Requisitos 
* Link do Desafio
```
https://github.com/bioritmo/front-end-code-challenge-smartsite
```
* Instalação do Angular 14
```
npm install -g @angular/cli 
```
### 🛠️ Ferramentas 
![Angular](https://img.shields.io/badge/angular-%23DD0031.svg?style=for-the-badge&logo=angular&logoColor=white)
![TypeScript](https://img.shields.io/badge/typescript-%23007ACC.svg?style=for-the-badge&logo=typescript&logoColor=white)
![SASS](https://img.shields.io/badge/SASS-hotpink.svg?style=for-the-badge&logo=SASS&logoColor=white)
### LINK
https://challengefrontend-smartfit.netlify.app/
![Captura de Tela (148)](https://github.com/PauloAquarius0299/desafio-frontend-smartfit/assets/114706743/ffcd32b2-1da9-4c22-86bd-8fff51940c23)
![Captura de Tela (149)](https://github.com/PauloAquarius0299/desafio-frontend-smartfit/assets/114706743/42621e8a-b8b4-42e3-977d-d5cfb8098f4b)


# ChallengeSmartfit

This project was generated with [Angular CLI](https://github.com/angular/angular-cli) version 14.2.13.

## Development server

Run `ng serve` for a dev server. Navigate to `http://localhost:4200/`. The application will automatically reload if you change any of the source files.

## Code scaffolding

Run `ng generate component component-name` to generate a new component. You can also use `ng generate directive|pipe|service|class|guard|interface|enum|module`.

## Build

Run `ng build` to build the project. The build artifacts will be stored in the `dist/` directory.

## Running unit tests

Run `ng test` to execute the unit tests via [Karma](https://karma-runner.github.io).

## Running end-to-end tests

Run `ng e2e` to execute the end-to-end tests via a platform of your choice. To use this command, you need to first add a package that implements end-to-end testing capabilities.

## Further help

To get more help on the Angular CLI use `ng help` or go check out the [Angular CLI Overview and Command Reference](https://angular.io/cli) page.
----- -- ---

import { Component, EventEmitter, OnInit, Output } from '@angular/core';
import { FormBuilder, FormGroup } from '@angular/forms';
import { GetUnitsService } from 'src/app/services/get-units.service';
import { Location } from '../../types/location.interface';
import { FilterUnitsService } from 'src/app/services/filter-units.service';


@Component({
  selector: 'app-forms',
  templateUrl: './forms.component.html',
  styleUrls: ['./forms.component.scss']
})
export class FormsComponent implements OnInit {
  @Output() submitEvent = new EventEmitter();
  results: Location[] = [];
  filteredResults: Location[] = [];
  formGroup!: FormGroup;

  constructor(
    private formBuilder: FormBuilder,
    private unitService: GetUnitsService,
    private filterUnitsService: FilterUnitsService

  ) { }

  ngOnInit(): void {
    this.formGroup = this.formBuilder.group({
      hour: '',
      showClosed: true
    })
    this.unitService.getAllUnits().subscribe(data => {
      this.results = data;
      this.filteredResults = data;
    })
  }

  onSubmit(): void {
    let {showClosed, hour} = this.formGroup.value
    this.filteredResults = this.filterUnitsService.filter(this.results, showClosed, hour);
    this.unitService.setFilteredUnits(this.filteredResults);

    this.submitEvent.emit();
  }

  onClean(): void{
    this.formGroup.reset();
  }

}

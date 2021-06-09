<template>
  <div>
    <div class="text-center" v-if="request">
      <div class="sk-circle">
        <div class="sk-circle1 sk-child"></div>
        <div class="sk-circle2 sk-child"></div>
        <div class="sk-circle3 sk-child"></div>
        <div class="sk-circle4 sk-child"></div>
        <div class="sk-circle5 sk-child"></div>
        <div class="sk-circle6 sk-child"></div>
        <div class="sk-circle7 sk-child"></div>
        <div class="sk-circle8 sk-child"></div>
        <div class="sk-circle9 sk-child"></div>
        <div class="sk-circle10 sk-child"></div>
        <div class="sk-circle11 sk-child"></div>
        <div class="sk-circle12 sk-child"></div>
      </div>
    </div>
    <div v-if="dataReady">
      <main class="wrapper-interior">
        <div class="content-general">
          <div class="max-width" v-if="dataReady">
            <form @submit.prevent="submitData">
              <h1 class="title-main">Ofertas</h1>
              <div
                id="deals_name"
                :class="[{ 'form-group--error': $v.nombre.$error }]"
              >
                <fieldset>
                  <legend>Nombre de la promoción</legend>
                  <p>
                    Este será el nombre de la promoción que veras en tu
                    calendario
                  </p>
                  <input
                    v-model="$v.nombre.$model"
                    class="form-control col48sm"
                    placeholder="Ej: Mes de la madre"
                  />
                  <span
                    v-if="$v.nombre.$error && !$v.nombre.required"
                    class="validation-error"
                  >
                    Nombre es requerido
                  </span>
                </fieldset>
              </div>

              <div id="deals_rate" class="card-details">
                <h2 class="preview-title">Ofertas</h2>
                <p>Para cada una de las opciones, puedes crear una promoción</p>
                <a @click="$modal.show('modal_deal')">
                  <svg
                    version="1.1"
                    xmlns="http://www.w3.org/2000/svg"
                    xmlns:xlink="http://www.w3.org/1999/xlink"
                    viewBox="0 0 45.958 45.958"
                    xml:space="preserve"
                    class="icon-l fill-primary"
                  >
                    <g>
                      <path
                        d="M22.979,0C10.288,0,0,10.289,0,22.979s10.288,22.979,22.979,22.979c12.69,0,22.979-10.289,22.979-22.979S35.67,0,22.979,0z M32.253,26.977h-5.301v5.289c0,2.207-1.765,3.996-3.972,3.996c-2.206,0-3.971-1.789-3.971-3.996v-5.291H13.71 c-2.207,0-4.006-1.789-4.006-3.997c0-2.207,1.796-3.996,4.003-3.996h5.302v-5.289c0-2.208,1.765-3.997,3.971-3.997 c2.208,0,3.972,1.789,3.972,3.997v5.29h5.301c2.207,0,3.997,1.79,3.997,3.997C36.25,25.188,34.46,26.977,32.253,26.977z"
                      />
                    </g>
                  </svg>
                  <span class="text-add">Agregar ofertas</span>
                </a>
                <modal
                  name="modal_deal"
                  width="600px"
                  height="auto"
                  :adaptive="true"
                  :scrollable="true"
                  :shiftX="0.5"
                  :shiftY="0.5"
                >
                  <div class="header-modal">
                    <div class="header-modal-title">Ofertas</div>
                    <button
                      type="button"
                      class="cross"
                      @click="$modal.hide('modal_deal')"
                      title="Cerrar"
                    >
                      X
                    </button>
                  </div>
                  <div class="overflow-div">
                    <div class="wrap-modal">
                      <div class="modal__deal">
                        <label class="bold label-form">Opciones</label>
                        <select class="form-control" v-model="selectedDeal">
                          <option disabled value="">
                            Selecciona una opción
                          </option>
                          <option
                            v-for="(option, index) in newOptions"
                            :key="index"
                            :value="index"
                          >
                            {{ option.nombre }}
                          </option>
                        </select>
                        <div
                          v-for="(option, index) in newOptions"
                          :key="index"
                          v-show="shouldDisplayDeal(index)"
                        >
                          <div
                            class="box-food"
                            v-for="(plan, index1) in option.planes"
                            :key="index1"
                            :id="`option_food_${plan.tarifaId}`"
                          >
                            <div class="checkbox-custom">
                              <input
                                type="checkbox"
                                :id="`checkbox_food_${plan.tarifaId}`"
                                :value="plan.tarifaId"
                                :checked="plan.tempSelected"
                                @change="plan.tempSelected = !plan.tempSelected"
                              />
                              <label :for="`checkbox_food_${plan.tarifaId}`">
                                {{ plan.tarifaNombre }}</label
                              >
                            </div>
                            <div class="mt-15" v-if="plan.tempSelected">
                              <p>
                                Escribe el valor del descuento que les ofrecerás
                                a tus clientes
                              </p>
                              <div class="fwp">
                                <div class="form-group mr-20">
                                  <label class="bold label-form">Moneda</label>
                                  <select
                                    class="form-control"
                                    v-model="plan.tarifaMoneda"
                                    disabled
                                  >
                                    <option
                                      v-for="(typeMoney, index) in typesMoney"
                                      :key="index"
                                      :value="lower(typeMoney)"
                                    >
                                      {{ typeMoney }}
                                    </option>
                                  </select>
                                </div>
                                <div
                                  :class="{
                                    'form-group--error':
                                      $v.newOptions.$each.$iter[index].planes
                                        .$each[index1].tipoDescuento.$error,
                                  }"
                                >
                                  <div class="form-group mr-20">
                                    <label class="bold label-form">Tipo</label>
                                    <select
                                      class="form-control"
                                      v-model="plan.tipoDescuento"
                                    >
                                      <option value="porcentaje">%</option>
                                      <option value="fijo">Fijo</option>
                                    </select>
                                  </div>
                                </div>

                                <div
                                  :class="{
                                    'form-group--error':
                                      $v.newOptions.$each.$iter[index].planes
                                        .$each[index1].valor.$error,
                                  }"
                                >
                                  <div class="form-group">
                                    <label class="bold label-form"
                                      >Valor descuento</label
                                    >
                                    <input
                                      type="number"
                                      v-model="plan.valor"
                                      min="0"
                                      name
                                      class="form-control wd-100"
                                      placeholder="Ej: 25000"
                                    />
                                  </div>

                                  <div
                                    v-if="
                                      $v.newOptions.$each.$iter[index].planes
                                        .$each[index1].valor.$error
                                    "
                                    class="validation-error"
                                  >
                                    <span
                                      v-if="
                                        !$v.newOptions.$each.$iter[index].planes
                                          .$each[index1].valor.numeric
                                      "
                                      >Numeric and minimum 0</span
                                    >

                                    <span
                                      v-else-if="
                                        !$v.newOptions.$each.$iter[index].planes
                                          .$each[index1].valor.maxValue
                                      "
                                      >MaxValue {{ plan.tarifaPrecio }}</span
                                    >
                                  </div>
                                </div>
                              </div>
                              <div
                                class="validation-error"
                                v-if="
                                  ($v.newOptions.$each.$iter[index].planes
                                    .$each[index1].valor.$error &&
                                    !$v.newOptions.$each.$iter[index].planes
                                      .$each[index1].valor.requiredIf) ||
                                  ($v.newOptions.$each.$iter[index].planes
                                    .$each[index1].tipoDescuento.$error &&
                                    !$v.newOptions.$each.$iter[index].planes
                                      .$each[index1].tipoDescuento.requiredIf)
                                "
                              >
                                Campos son requeridos
                              </div>
                            </div>
                          </div>
                        </div>
                        <div
                          class="validation-error"
                          v-if="
                            selectedDeal != -1 &&
                            $v.newOptions.$each.$iter[selectedDeal].planes
                              .$each[0].tempSelected.$error &&
                            !$v.newOptions.$each.$iter[selectedDeal].planes
                              .$each[0].tempSelected.checked
                          "
                        >
                          Please select at least one option.
                        </div>
                        <div class="mt-15">
                          <button
                            type="button"
                            class="btn btn-save btn--blue"
                            @click="addNewOffer()"
                          >
                            Guardar
                          </button>
                        </div>
                      </div>
                    </div>
                  </div>
                </modal>
                <!-- eslint-disable -->
                <div class="table-div mt-15">
                  <table class="table table-js">
                    <thead class="tablehead-js">
                      <tr>
                        <th>Opciones</th>
                        <th>Plan de tarifas</th>
                        <th>Moneda</th>
                        <th>Tipo</th>
                        <th>Valor</th>
                      </tr>
                    </thead>
                    <tbody>
                      <v-collapse-group ref="opt_group" :onlyOneActive="true">
                        <tr
                          v-if="opt.selected"
                          v-for="(opt, idx) in newOptions"
                          :key="idx"
                        >
                          <td colspan="5" class="p-0 brd-0">
                            <v-collapse-wrapper
                              v-on:onStatusChange="activeCollapse"
                              :ref="'opt_group_' + opt.opcion"
                            >
                              <table>
                                <tr class="tablerow-js">
                                  <td colspan="5" class="text-left">
                                    <div v-collapse-toggle>
                                      <button
                                        type="button"
                                        class="btn__expando-row link-button"
                                      >
                                        <svg
                                          version="1.1"
                                          xmlns="http://www.w3.org/2000/svg"
                                          xmlns:xlink="http://www.w3.org/1999/xlink"
                                          x="0px"
                                          y="0px"
                                          width="451.847px"
                                          height="451.847px"
                                          viewBox="0 0 451.847 451.847"
                                          style="
                                            enable-background: new 0 0 451.847
                                              451.847;
                                          "
                                          xml:space="preserve"
                                          focusable="false"
                                          class="icon-xs fill-primary arrow-svg"
                                        >
                                          <g>
                                            <path
                                              d="M225.923,354.706c-8.098,0-16.195-3.092-22.369-9.263L9.27,151.157c-12.359-12.359-12.359-32.397,0-44.751c12.354-12.354,32.388-12.354,44.748,0l171.905,171.915l171.906-171.909c12.359-12.354,32.391-12.354,44.744,0c12.365,12.354,12.365,32.392,0,44.751L248.292,345.449C242.115,351.621,234.018,354.706,225.923,354.706z"
                                            />
                                          </g>
                                        </svg>
                                        {{ opt.nombre }}
                                      </button>
                                    </div>
                                  </td>
                                </tr>
                                <tr>
                                  <td colspan="5" class="p-0 brd-0">
                                    <div
                                      class="accordion__content"
                                      v-collapse-content
                                    >
                                      <template v-for="plan in opt.planes">
                                        <tr
                                          class="tablerow-js"
                                          v-if="plan.selected"
                                        >
                                          <td></td>
                                          <td>{{ plan.tarifaNombre }}</td>
                                          <td>{{ plan.tarifaMoneda }}</td>
                                          <td>{{ plan.tipoDescuento }}</td>
                                          <td>{{ plan.valor }}</td>
                                        </tr>
                                      </template>
                                    </div>
                                  </td>
                                </tr>
                              </table>
                            </v-collapse-wrapper>
                          </td>
                        </tr>
                      </v-collapse-group>
                    </tbody>
                  </table>
                </div>
                <!-- eslint-enable -->
              </div>

              <div id="deals_dates">
                <fieldset>
                  <legend>Fechas</legend>
                  <p>Selecciona las fechas de la promoción</p>
                  <div
                    :class="[
                      'calendar__dates',
                      {
                        'form-group--error':
                          $v.fechaDesde.$error || $v.fechaHasta.$error,
                      },
                    ]"
                  >
                    <div class="mb-15 mr-20">
                      <label class="bold label-form">Fechas *</label>
                      <Calendar
                        id="accomodations"
                        v-model="$v.fechaDesde.$model"
                        :disabledDates="disabledDates"
                        :from="fechaDesde"
                        :to="fechaHasta"
                      />
                      <span
                        v-if="
                          ($v.fechaDesde.$error && !$v.fechaDesde.required) ||
                          ($v.fechaHasta.$error && !$v.fechaHasta.required)
                        "
                        class="validation-error"
                      >
                        Fechas es requerido
                      </span>
                    </div>
                  </div>
                  <p>¿A qué días de la semana desea aplicar los cambios?</p>
                  <div class="mb-15 fwp">
                    <div
                      class="checkbox-custom wd-130"
                      v-for="(calendarDay, index) in calendarDays"
                      :key="index"
                    >
                      <input
                        type="checkbox"
                        v-model="calendarDay.check"
                        :id="`checkbox_calendar_${calendarDay.nombre}`"
                        :value="calendarDay.nombre"
                      />
                      <label :for="`checkbox_calendar_${calendarDay.nombre}`">{{
                        calendarDay.nombre
                      }}</label>
                    </div>
                  </div>
                  <div class="help-block" v-if="fechaDesde && fechaHasta">
                    Los cambios serán realizados entre el
                    <span id="date_from">{{ formatDate(fechaDesde) }}</span> y
                    el
                    <span id="date_to">{{ formatDate(fechaHasta) }}</span>
                  </div>
                </fieldset>
              </div>
              <div class="form__save">
                <button type="submit" class="btn btn--blue mr-10">
                  Guardar
                </button>
                <button
                  @click="previousPage()"
                  type="button"
                  class="btn btn--white"
                >
                  Cancelar
                </button>
              </div>
            </form>
          </div>
        </div>
      </main>
    </div>
  </div>
</template>

<script>
import service from "@/services/service.js";
import Calendar from "@/components/Calendar";
import { required, numeric, requiredIf } from "vuelidate/lib/validators";
import { EventBus } from "@/bus";
import format from "date-fns/format";
import shared from "@/shared";

const cMaxValue = function (value, obj) {
  if (!value) return true;
  return value <= obj.tarifaPrecio;
};

export default {
  components: {
    Calendar,
  },
  data() {
    return {
      dataReady: false,
      request: false,
      selectedDeal: -1,
      disabledDates: [],
      nombre: "",
      deals: [],
      allDeals: [],
      fechaDesde: null,
      fechaHasta: null,
      newOptions: [],
      ofertaTarifas: [
        {
          moneda: "",
          tarifa: "",
          tarifa_id: "",
          tipo_descuento: "",
          valor: "",
        },
      ],
      typesMoney: [],
      calendarDays: [
        { nombre: "Lunes", check: "true" },
        { nombre: "Martes", check: "true" },
        { nombre: "Miercoles", check: "true" },
        { nombre: "Jueves", check: "true" },
        { nombre: "Viernes", check: "true" },
        { nombre: "Sabado", check: "true" },
        { nombre: "Domingo", check: "true" },
      ],
      links: {},
    };
  },
  provide() {
    return {
      $v: this.$v,
    };
  },
  validations: {
    nombre: {
      required,
    },
    fechaDesde: {
      required,
    },
    fechaHasta: {
      required,
    },
    newOptions: {
      $each: {
        planes: {
          $each: {
            tempSelected: {
              checked() {
                if (this.selectedDeal == -1) {
                  return true;
                }
                var selectedPlans = [];
                for (var $i = 0; $i < this.newOptions.length; $i++) {
                  if ($i == this.selectedDeal) {
                    selectedPlans = this.newOptions[$i].planes;
                    break;
                  }
                }
                var checked = selectedPlans.filter((x) => x.tempSelected);
                return checked.length >= 1 ? true : false;
              },
            },
            tipoDescuento: {
              requiredIf: requiredIf((object) => object.tempSelected),
            },
            valor: {
              requiredIf: requiredIf((object) => object.tempSelected),
              numeric,
              maxValue: cMaxValue,
            },
          },
        },
      },
    },
  },
  created() {
    var ths = this;
    EventBus.$on("date-from", (data) => {
      ths.fechaDesde = data;
    });
    EventBus.$on("date-to", (data) => {
      ths.fechaHasta = data;
    });
  },
  async beforeMount() {
    this.request = true;
    const response6 = await service.get(`services-options/types-money`);
    this.typesMoney = response6.data;

    if (this.$route.name == "DealsCreate") {
      const response7 = await service.get(
        `category?service_id=${this.$route.params.id}`
      );
      this.category = response7.data;
      this.menuname = this.category.nombre;
      this.linksGeneration(this.$route.params.id, this.menuname);

      const response8 = await service.get(
        `deals/services-options?service_id=${this.$route.params.id}`
      );
      this.options = response8.data;

      const response5 = await service.get(
        `deals?service_id=${this.$route.params.id}`
      );
      this.allDeals = response5.data;

      let newOptions = this.options.map((option) => {
        const container = {};
        container.opcion = option.id;
        container.nombre = option.nombre;
        container.planes = [];
        container.selected = false;
        option.planes.forEach((plan) => {
          let dealObj = {
            tarifaId: plan.id,
            tarifaNombre: plan.nombre,
            tarifaPrecio: plan.precio,
            tarifaMoneda: option.moneda,
            selected: false,
            tempSelected: false,
          };
          let foundIndex = container.planes.findIndex(
            (el) => el.tarifaId === dealObj.tarifaId
          );
          if (foundIndex === -1) {
            container.planes.push(dealObj);
          }
        });
        return container;
      });
      this.newOptions = newOptions;

      this.getDisabledDates();

      this.dataReady = true;
      this.request = false;
    } else {
      this.getData();
    }
  },
  updated: function () {
    this.$nextTick(function () {
      shared.adjustTableWidth();
    });
  },
  metaInfo: {
    title: "Ofertas",
    titleTemplate: "%s - Viaja y Descubre",
  },
  methods: {
    async getData() {
      const response = await service.get(`deals/${this.$route.params.id}`);
      this.deals = response.data;

      const response2 = await service.get(
        `deals/search-service/${this.$route.params.id}`
      );
      this.serviceId = response2.data;

      const response3 = await service.get(
        `deals/services-options?service_id=${this.serviceId[0].servicio}`
      );
      this.options = response3.data;

      const response4 = await service.get(
        `category?service_id=${this.serviceId[0].servicio}`
      );
      this.category = response4.data;
      this.menuname = this.category.nombre;
      this.linksGeneration(this.serviceId[0].servicio, this.menuname);

      const response5 = await service.get(
        `deals?service_id=${this.serviceId[0].servicio}`
      );
      this.allDeals = response5.data;

      this.nombre = this.deals.nombre;
      this.fechaDesde = this.deals.fechaDesde;
      this.fechaHasta = this.deals.fechaHasta;

      let calendarDays = [
        { nombre: "Lunes", check: this.deals.lunes },
        { nombre: "Martes", check: this.deals.martes },
        { nombre: "Miercoles", check: this.deals.miercoles },
        { nombre: "Jueves", check: this.deals.jueves },
        { nombre: "Viernes", check: this.deals.viernes },
        { nombre: "Sabado", check: this.deals.sabado },
        { nombre: "Domingo", check: this.deals.domingo },
      ];
      this.calendarDays = calendarDays;

      this.ofertaTarifas = this.deals.ofertaTarifas;

      let newOptions = this.options.map((option) => {
        const container = {};
        container.opcion = option.id;
        container.nombre = option.nombre;
        container.selected = false;
        container.planes = [];
        let dealObj = {};
        option.planes.forEach((plan) => {
          this.ofertaTarifas.forEach((e) => {
            if (plan.id == e.tarifaBase.id) {
              container.selected = true;
              dealObj = {
                tarifaId: plan.id,
                tarifaNombre: plan.nombre,
                tarifaPrecio: plan.precio,
                ofertaTarifaId: e.tarifaBase.id,
                tipoDescuento: e.tipoDescuento,
                valor: e.valor,
                tarifaMoneda: option.moneda,
                ofertaId: e.oferta,
                oferta: true,
                selected: false,
                tempSelected: false,
              };
              let foundIndex = container.planes.findIndex(
                (el) => el.tarifaId === dealObj.tarifaId
              );
              if (foundIndex === -1) {
                dealObj.selected = this.isPlanSelected(dealObj);
                dealObj.tempSelected = this.isPlanSelected(dealObj);
                container.planes.push(dealObj);
              }
            }
          });
          if (plan.oferta != true) {
            dealObj = {
              tarifaId: plan.id,
              tarifaNombre: plan.nombre,
              tarifaPrecio: plan.precio,
              tarifaMoneda: option.moneda,
              selected: false,
              tempSelected: false,
              valor: "",
            };
            let foundIndex = container.planes.findIndex(
              (el) => el.tarifaId === dealObj.tarifaId
            );
            if (foundIndex === -1) {
              dealObj.selected = this.isPlanSelected(dealObj);
              dealObj.tempSelected = this.isPlanSelected(dealObj);
              container.planes.push(dealObj);
            }
          }
        });
        return container;
      });
      this.newOptions = newOptions;

      this.getDisabledDates();

      this.dataReady = true;
      this.request = false;
    },
    async submitData() {
      var isInvalid = false;

      for (var param in this.$v.$params) {
        if (param != "newOptions") {
          this.$v[param].$touch();

          if (this.$v[param].$invalid) {
            isInvalid = true;
            break;
          }
        } else {
          var selected = this.newOptions.filter((x) => x.selected);
          if (!selected.length) {
            this.$v[param].$touch();
            this.$modal.show("modal_deal");
            isInvalid = true;
            break;
          }
        }
      }

      if (isInvalid) {
        return false;
      } else {
        //   let data;
        let body = {
          nombre: this.nombre,
          fechaDesde: this.fechaDesde,
          fechaHasta: this.fechaHasta,
          calendarDays: this.calendarDays,
        };

        let tarifas = [];
        this.newOptions.forEach((option) => {
          if (option.selected) {
            option.planes.forEach((plan) => {
              if (plan.selected)
                tarifas.push({
                  moneda: plan.tarifaMoneda,
                  tarifa: plan.tarifaNombre,
                  tarifa_id: plan.tarifaId,
                  tipo_descuento: plan.tipoDescuento,
                  valor: plan.valor,
                });
            });
          }
        });

        body["tarifas"] = tarifas;
        console.log(JSON.stringify(body));
        /* 
        if (this.$route.name == "DealsCreate") {
          try {
            data = await service.post(`deals/create`, body);
          } catch (e) {
            console.log(e.data);
          }
        } else {
          try {
            data = await service.post(
              `deals/edit/${this.$route.params.id}`,
              body
            );
          } catch (e) {
            console.log(e.data);
          }
        }
        console.log(data); */
        //push to service Id
        /*  this.$router.push({ name: "DealsList", params: { id: 1 } }); */
      }
    },
    getDisabledDates() {
      var disabledDates = [];

      for (var deal in this.allDeals) {
        var range = shared.getDates(
          this.allDeals[deal].fechaDesde,
          this.allDeals[deal].fechaHasta,
          "YYYY-MM-DD"
        );

        disabledDates = disabledDates.concat(range);
      }

      this.disabledDates = disabledDates;
    },
    isPlanSelected(plan) {
      return plan.valor && plan.tipoDescuento ? true : false;
    },
    addNewOffer() {
      if (this.selectedDeal >= 0) {
        this.$v.newOptions.$touch();
        if (this.$v.newOptions.$invalid) {
          return;
        }
      }

      for (var $i = 0; $i < this.newOptions.length; $i++) {
        if (this.selectedDeal == $i) {
          this.newOptions[$i].selected = true;

          for (var $j = 0; $j < this.newOptions[$i].planes.length; $j++) {
            if (this.newOptions[$i].planes[$j].tempSelected) {
              this.newOptions[$i].planes[$j].selected = true;
            } else {
              this.newOptions[$i].planes[$j].selected = false;
            }
          }

          break;
        }
      }

      if (this.selectedDeal >= 0) {
        for ($i = 0; $i < this.newOptions.length; $i++) {
          if (this.selectedDeal == $i) {
            var ths = this;
            ths.$nextTick(function () {
              ths.$refs["opt_group_" + ths.newOptions[$i].opcion][0].open();
            });
            break;
          }
        }
      }

      this.$modal.hide("modal_deal");
    },
    shouldDisplayDeal: function (value) {
      return this.selectedDeal === value;
    },
    lower(val) {
      return val.toLowerCase();
    },
    previousPage() {
      this.$router.back();
    },
    formatDate(date) {
      var rdate = "";
      if (date) {
        rdate =
          format(date, "D") +
          " de " +
          " " +
          format(date, "MMMM").toLowerCase() +
          " de " +
          format(date, "YYYY");
      }
      return rdate;
    },
    activeCollapse: function (e) {
      if (true == e.status) {
        return (e.vm.nodes.toggle.className =
          "v-collapse-toggler active_collapse");
      } else {
        return (e.vm.nodes.toggle.className = "v-collapse-toggler");
      }
    },
    linksGeneration(id, name = null) {
      name = name ? name : "";
      this.links = {
        listado: { name: name + "List" },
        detalles: { name: name + "Edit", params: { id: id } },
        opciones: { name: name + "ListOptions", params: { id: id } },
        tarifas: { name: name + "Calendar", params: { id: id } },
        planTarifas: { name: "RatesList", params: { id: id } },
        condiciones: { name: "Conditions", params: { id: id } },
        ofertas: { name: "DealsList", params: { id: id } },
        comentarios: { name: "Review", params: { id: id } },
      };
    },
  },
};
</script>

<style scoped>
.btn__expando-row {
  width: auto;
  cursor: default;
  font-weight: 600;
}
.preview-title {
  margin-bottom: 15px;
  font-size: 1.25em;
}
.box-food {
  padding: 15px 10px 5px 0px;
  border-bottom: solid 1px var(--color-background-light);
}
.v-collapse-toggler {
  width: auto;
  display: inline-block;
}
.v-collapse-toggler button {
  cursor: pointer;
}
.v-collapse-toggler:after {
  display: none;
}
.active_collapse .arrow-svg {
  transform: rotate(180deg);
}
.v-collapse-toggler {
  background: transparent;
  padding: 0;
}
.vc-collapse {
  border: none;
  box-shadow: none;
}
</style>
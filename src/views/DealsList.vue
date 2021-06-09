<template>
  <div>
    <div v-if="dataReady">
      <main class="wrapper-interior">
        <div class="content-general">
          <div class="max-width">
            <div class="title-button">
              <h1 class="title-button__title">Listado Ofertas</h1>
              <router-link
                :to="{
                  name: 'DealsCreate',
                }"
              >
                <button class="btn btn--blue mb-15">
                  <svg
                    version="1.1"
                    xmlns="http://www.w3.org/2000/svg"
                    xmlns:xlink="http://www.w3.org/1999/xlink"
                    viewBox="0 0 45.958 45.958"
                    xml:space="preserve"
                    class="icon-l fill-light"
                  >
                    <g>
                      <path
                        d="M22.979,0C10.288,0,0,10.289,0,22.979s10.288,22.979,22.979,22.979c12.69,0,22.979-10.289,22.979-22.979S35.67,0,22.979,0z M32.253,26.977h-5.301v5.289c0,2.207-1.765,3.996-3.972,3.996c-2.206,0-3.971-1.789-3.971-3.996v-5.291H13.71 c-2.207,0-4.006-1.789-4.006-3.997c0-2.207,1.796-3.996,4.003-3.996h5.302v-5.289c0-2.208,1.765-3.997,3.971-3.997 c2.208,0,3.972,1.789,3.972,3.997v5.29h5.301c2.207,0,3.997,1.79,3.997,3.997C36.25,25.188,34.46,26.977,32.253,26.977z"
                      />
                    </g>
                  </svg>
                  <span class="text-icon">Crear Oferta</span>
                </button>
              </router-link>
            </div>
            <div class="card-details">
              Crea y edita ofertas para cada una de las opciones disponibles
            </div>
            <div class="table-div">
              <div class="col48sm mb-15">
                <label class="label-form semi-bold" for="searchBoxTable"
                  >Filtrar</label
                >
                <input
                  type="search"
                  id="searchBoxTable"
                  class="form-control"
                  placeholder="Puedes filtrar por Id, Plan de tarifa..."
                />
              </div>
              <div class="table-responsive">
                <table class="table wd-100p table-js">
                  <thead class="stickytable tablehead-js">
                    <tr>
                      <th width="70">Id</th>
                      <th width="50%">Detalle</th>
                      <th>Moneda</th>
                      <th>Tipo</th>
                      <th>Descuento</th>
                    </tr>
                  </thead>
                  <tbody>
                    <!-- eslint-disable -->
                    <template v-for="(deal, index) in allDeals">
                      <tr>
                        <td colspan="5" class="p-0 brd-0">
                          <v-collapse-wrapper
                            v-on:onStatusChange="activeCollapse"
                          >
                            <table>
                              <tr class="tablerow-js">
                                <td class="td__expando-row brd-r-0" colspan="4">
                                  <div v-collapse-toggle>
                                    <button
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
                                      {{ deal.nombre }} -
                                      {{ formatDate(deal.fechaDesde) }}/{{
                                        formatDate(deal.fechaHasta)
                                      }}
                                    </button>
                                  </div>
                                </td>
                                <td class="brd-l-0">
                                  <a
                                    class="deals__expando-row"
                                    title="Eliminar"
                                  >
                                    <svg
                                      height="512pt"
                                      viewBox="-57 0 512 512"
                                      width="512pt"
                                      xmlns="http://www.w3.org/2000/svg"
                                      class="icon-xl fill-muted"
                                    >
                                      <g>
                                        <path
                                          d="m156.371094 30.90625h85.570312v14.398438h30.902344v-16.414063c.003906-15.929687-12.949219-28.890625-28.871094-28.890625h-89.632812c-15.921875 0-28.875 12.960938-28.875 28.890625v16.414063h30.90625zm0 0"
                                        />
                                        <path
                                          d="m344.210938 167.75h-290.109376c-7.949218 0-14.207031 6.78125-13.566406 14.707031l24.253906 299.90625c1.351563 16.742188 15.316407 29.636719 32.09375 29.636719h204.542969c16.777344 0 30.742188-12.894531 32.09375-29.640625l24.253907-299.902344c.644531-7.925781-5.613282-14.707031-13.5625-14.707031zm-219.863282 312.261719c-.324218.019531-.648437.03125-.96875.03125-8.101562 0-14.902344-6.308594-15.40625-14.503907l-15.199218-246.207031c-.523438-8.519531 5.957031-15.851562 14.472656-16.375 8.488281-.515625 15.851562 5.949219 16.375 14.472657l15.195312 246.207031c.527344 8.519531-5.953125 15.847656-14.46875 16.375zm90.433594-15.421875c0 8.53125-6.917969 15.449218-15.453125 15.449218s-15.453125-6.917968-15.453125-15.449218v-246.210938c0-8.535156 6.917969-15.453125 15.453125-15.453125 8.53125 0 15.453125 6.917969 15.453125 15.453125zm90.757812-245.300782-14.511718 246.207032c-.480469 8.210937-7.292969 14.542968-15.410156 14.542968-.304688 0-.613282-.007812-.921876-.023437-8.519531-.503906-15.019531-7.816406-14.515624-16.335937l14.507812-246.210938c.5-8.519531 7.789062-15.019531 16.332031-14.515625 8.519531.5 15.019531 7.816406 14.519531 16.335937zm0 0"
                                        />
                                        <path
                                          d="m397.648438 120.0625-10.148438-30.421875c-2.675781-8.019531-10.183594-13.429687-18.640625-13.429687h-339.410156c-8.453125 0-15.964844 5.410156-18.636719 13.429687l-10.148438 30.421875c-1.957031 5.867188.589844 11.851562 5.34375 14.835938 1.9375 1.214843 4.230469 1.945312 6.75 1.945312h372.796876c2.519531 0 4.816406-.730469 6.75-1.949219 4.753906-2.984375 7.300781-8.96875 5.34375-14.832031zm0 0"
                                        />
                                      </g>
                                    </svg>
                                  </a>
                                  <a
                                    class="deals__expando-row"
                                    title="Desactivar"
                                  >
                                    <svg
                                      height="512pt"
                                      viewBox="0 0 512 512"
                                      width="512pt"
                                      xmlns="http://www.w3.org/2000/svg"
                                      class="icon-xl fill-danger mr-5"
                                    >
                                      <path
                                        d="m437.019531 74.980469c-48.351562-48.351563-112.640625-74.980469-181.019531-74.980469s-132.667969 26.628906-181.019531 74.980469c-48.351563 48.351562-74.980469 112.640625-74.980469 181.019531 0 68.382812 26.628906 132.667969 74.980469 181.019531 48.351562 48.351563 112.640625 74.980469 181.019531 74.980469s132.667969-26.628906 181.019531-74.980469c48.351563-48.351562 74.980469-112.636719 74.980469-181.019531 0-68.378906-26.628906-132.667969-74.980469-181.019531zm-70.292969 256.386719c9.761719 9.765624 9.761719 25.59375 0 35.355468-4.882812 4.882813-11.28125 7.324219-17.679687 7.324219s-12.796875-2.441406-17.679687-7.324219l-75.367188-75.367187-75.367188 75.371093c-4.882812 4.878907-11.28125 7.320313-17.679687 7.320313s-12.796875-2.441406-17.679687-7.320313c-9.761719-9.765624-9.761719-25.59375 0-35.355468l75.371093-75.371094-75.371093-75.367188c-9.761719-9.765624-9.761719-25.59375 0-35.355468 9.765624-9.765625 25.59375-9.765625 35.355468 0l75.371094 75.367187 75.367188-75.367187c9.765624-9.761719 25.59375-9.765625 35.355468 0 9.765625 9.761718 9.765625 25.589844 0 35.355468l-75.367187 75.367188zm0 0"
                                      />
                                    </svg>
                                  </a>
                                  <a class="deals__expando-row" title="Editar">
                                    <router-link
                                      :to="{
                                        name: 'DealsEdit',
                                        params: { id: deal.id },
                                      }"
                                    >
                                      <svg
                                        version="1.1"
                                        xmlns="http://www.w3.org/2000/svg"
                                        xmlns:xlink="http://www.w3.org/1999/xlink"
                                        x="0px"
                                        y="0px"
                                        viewBox="0 0 512 512"
                                        style="
                                          enable-background: new 0 0 512 512;
                                        "
                                        xml:space="preserve"
                                        class="icon-xl fill-primary mr-5"
                                      >
                                        <g>
                                          <g>
                                            <g>
                                              <path
                                                d="M352.459,220c0-11.046-8.954-20-20-20h-206c-11.046,0-20,8.954-20,20s8.954,20,20,20h206C343.505,240,352.459,231.046,352.459,220z"
                                              />
                                              <path
                                                d="M126.459,280c-11.046,0-20,8.954-20,20c0,11.046,8.954,20,20,20H251.57c11.046,0,20-8.954,20-20c0-11.046-8.954-20-20-20H126.459z"
                                              />
                                              <path
                                                d="M173.459,472H106.57c-22.056,0-40-17.944-40-40V80c0-22.056,17.944-40,40-40h245.889c22.056,0,40,17.944,40,40v123c0,11.046,8.954,20,20,20c11.046,0,20-8.954,20-20V80c0-44.112-35.888-80-80-80H106.57c-44.112,0-80,35.888-80,80v352c0,44.112,35.888,80,80,80h66.889c11.046,0,20-8.954,20-20C193.459,480.954,184.505,472,173.459,472z"
                                              />
                                              <path
                                                d="M467.884,289.572c-23.394-23.394-61.458-23.395-84.837-0.016l-109.803,109.56c-2.332,2.327-4.052,5.193-5.01,8.345l-23.913,78.725c-2.12,6.98-0.273,14.559,4.821,19.78c3.816,3.911,9,6.034,14.317,6.034c1.779,0,3.575-0.238,5.338-0.727l80.725-22.361c3.322-0.92,6.35-2.683,8.79-5.119l109.573-109.367C491.279,351.032,491.279,312.968,467.884,289.572zM333.776,451.768l-40.612,11.25l11.885-39.129l74.089-73.925l28.29,28.29L333.776,451.768z M439.615,346.13l-3.875,3.867l-28.285-28.285l3.862-3.854c7.798-7.798,20.486-7.798,28.284,0C447.399,325.656,447.399,338.344,439.615,346.13z"
                                              />
                                              <path
                                                d="M332.459,120h-206c-11.046,0-20,8.954-20,20s8.954,20,20,20h206c11.046,0,20-8.954,20-20S343.505,120,332.459,120z"
                                              />
                                            </g>
                                          </g>
                                        </g>
                                      </svg>
                                    </router-link>
                                  </a>
                                </td>
                              </tr>
                              <tr>
                                <td colspan="5" class="p-0 brd-0">
                                  <div
                                    class="accordion__content"
                                    v-collapse-content
                                  >
                                    <template
                                      v-for="deal in allDeals[index]
                                        .ofertaTarifas"
                                    >
                                      <tr class="tablerow-js">
                                        <td>{{ deal.oferta }}</td>
                                        <td>
                                          {{
                                            deal.tarifaBase.opcionServicio
                                              .nombre
                                          }},
                                          {{ deal.tarifaBase.nombre }}
                                        </td>
                                        <td>{{ deal.moneda }}</td>
                                        <td>
                                          {{ deal.tipoDescuento }}
                                        </td>
                                        <td>{{ deal.valor }}</td>
                                      </tr>
                                    </template>
                                  </div>
                                </td>
                              </tr>
                            </table>
                          </v-collapse-wrapper>
                        </td>
                      </tr>
                    </template>
                    <!-- eslint-enable -->
                  </tbody>
                </table>
              </div>
            </div>
          </div>
        </div>
      </main>
    </div>
  </div>
</template>

<script>
import service from "@/services/service.js";
import format from "date-fns/format";
import shared from "@/shared";

export default {
  components: {},
  data() {
    return {
      dataReady: false,
      allDeals: [],
      menuname: "",
      links: {},
      dateFormat: "D MMM YYYY",
    };
  },
  async beforeMount() {
    this.getData();
  },
  updated: function () {
    this.$nextTick(function () {
      console.log("as");
      shared.adjustTableWidth();
    });
  },
  metaInfo: {
    title: "Listado Ofertas",
    titleTemplate: "%s - Viaja y Descubre",
  },
  methods: {
    async getData() {
      const response = await service.get(
        `deals?service_id=${this.$route.params.id}`
      );
      this.allDeals = response.data;

      const response2 = await service.get(
        `category?service_id=${this.$route.params.id}`
      );

      this.category = response2.data;
      this.menuname = this.category.nombre;
      this.linksGeneration(this.$route.params.id, this.menuname);
      this.dataReady = true;
    },
    activeCollapse: function (e) {
      if (true == e.status) {
        return (e.vm.nodes.toggle.className =
          "v-collapse-toggler active_collapse");
      } else {
        return (e.vm.nodes.toggle.className = "v-collapse-toggler");
      }
    },
    formatDate(date) {
      return format(date, this.dateFormat);
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
.deals__expando-row {
  float: right;
}
.td__expando-row {
  text-align: left;
}
.btn__expando-row {
  font-weight: 600;
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
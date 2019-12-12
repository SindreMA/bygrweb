<template>
  <div>
    <h6 class="noMargin">Building A - H{{data.residentialUnitGroupId}}</h6>
    <div id="Container" class="topMargin shadow-1 flex">
      <div id="dragMark">
        <div class="smallCircle"></div>
        <div class="smallCircle"></div>
        <div class="smallCircle"></div>
      </div>
      <div class="flexNoGrow">
        <div>
          <div class="greyText">Floor</div>
          <div>{{data.floor}}</div>
        </div>
        <div>
          <div class="greyText">Unit type</div>
          <div>{{data.type}}</div>
        </div>
        <div>
          <div class="greyText">Type</div>
          <div>{{data.layoutType}}</div>
        </div>
      </div>
      <div class="flexNoGrow" v-for="(buyer, index) in data.buyers" :key="buyer.key">
        <div class="greyText flexNoGrow">Buyer {{index + 1}}</div>
        <div>{{buyer.displayName}}</div>
        <div>{{buyer.phoneNumber}}</div>
        <div>{{buyer.email}}</div>
        <!--<div>Invited: {{getDateFormat(buyer.lastVisitDate)}}</div>-->
        <div>
          Invited: xx.xx.xxxx
          <q-tooltip>JSON filen manglet denne informasjonen</q-tooltip>
        </div>
      </div>
      <div class="flexNoGrow">
        <div class="greyText">Last login</div>
        <div>{{lastLoginBuyer.displayName}}</div>
        <div>{{getDateTimeFormat(lastLoginBuyer.lastVisitDate)}}</div>
      </div>
      <div class="flexGrow">
        <div class="greyText">Deadlines</div>
        <div v-for="(deadline,index) in data.deadlines" :key="index">
          <div class="flex">
            <div>{{deadline.name}} ({{getDateFormat(deadline.date)}}):</div>
            <statusLine :status="deadline.status" />
          </div>
        </div>
      </div>
    </div>
    <br />
  </div>
</template>

<script>
import moment from "moment";
import _ from "underscore";
import statusLine from "../components/statusLine";

export default {
  props: ["data"],
  methods: {
    getDateFormat(date) {
      return moment(date).format("DD.MM.YYYY");
    },
    getDateTimeFormat(date) {
      return moment(date).format("DD.MM.YYYY hh:mm");
    }
  },
  computed: {
    lastLoginBuyer() {
      return _.sortBy(this.data.buyers, x => {
        return moment(x.lastVisitDate).unix();
      }).reverse()[0];
    }
  },
  components: {
    statusLine
  }
};
</script>

<style scouped>
#Container {
  border-radius: 5px;
  padding: 12px;

  position: relative;
}
.noMargin {
  margin: 0;
}
.greyText {
  color: grey;
}
.topMargin * {
  margin-top: 3px;
}
.flexGrow {
  flex-grow: 2;
}
.flexNoGrow {
  flex-grow: 1;
}
.smallCircle {
  width: 4px;
  height: 4px;
  background-color: grey;
  border-radius: 100%;
  position: relative;
  margin-top: 4px;
}
#dragMark {
  position: absolute;
  right: 15px;
  top: 40%;
}
#dragMark:hover {
  cursor: grab;
}
</style>

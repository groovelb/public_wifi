<template>
  <Container :isPop="isPop">
    <Section ref="top">
      <button v-on:click="togglePop" class="close">✕</button>
      <h3>버스 노선 현황 정보</h3>
      <SectionRow>
        <CheckContainer v-for="(route, index) in busRouteList" :key="index">
          <input
            type="checkbox"
            :id="route.number"
            :value="route.number"
            v-model="checkedNames"
          />
          <label :for="route.number">
            {{ `${route.number}` }}
          </label>
        </CheckContainer>
      </SectionRow>
      <MeasureTime>
        <div>
          측정시간: <span>{{ measureTime }}</span>
        </div>
        <button>
          <img :src="ic_refrsh" alt="" />
        </button>
      </MeasureTime>
    </Section>
    <TableSection :topHeight="topHeight" :bottomHeight="bottomHeight">
      <KeyIndicatorTable :tableData="routeDataDummyList" />
    </TableSection>
    <Section ref="bottom">
      <LegendIndicator />
    </Section>
  </Container>
</template>
<script>
import styled from "vue-styled-components";
import { TYPE, CODE, SCALE } from "../../../globalConst/indicatorCode";
import KeyIndicatorTable from "@/components/table/KeyIndicatorTable/KeyIndicatorTable";
import LegendIndicator from "@/components/table/KeyIndicatorTable/LegendIndicator";

// Assets
import ic_refrsh from "../../../assets/icon/action/refresh.svg";

const ContainerProps = { isPop: Boolean };

const Container = styled("div", ContainerProps)`
  width: 680px;
  padding: 24px;
  padding-top: 96px;
  height: 100%;
  background-color: #fff;
  transition: all 0.3s ease-in;
  position: fixed;
  top: 0;
  left: ${(props) => (props.isPop ? "104px" : "-728px")};
  z-index: 99;
  text-align: left;
  button.close {
    position: absolute;
    top: 72px;
    right: 8px;
    width: 48px;
    height: 48px;
    /* border-radius: 50%; */
    outline: none;
    border: none;
    background-color: #fff;
    color: ${(props) => props.theme.color.ui.middle2};
    ${(props) => props.theme.layout.flexColCenter}
    font-size: 20px;
  }
  @media only screen and (max-width: 480px) {
    padding-top: 32px;
    left: ${(props) => (props.isPop ? "0px" : "-728px")};
    width: calc(100% - 32px);
    padding: 16px;
    overflow-y: overlay;
    button.close {
      top: 12px;
      right: 12px;
    }
  }
`;

const Section = styled.div`
  width: 100%;
`;

const MeasureTime = styled.div`
  width: 100%;
  height: 48px;
  /* justify-content: space-between; */
  display: flex;
  align-items: center;
  span {
    margin-left: 8px;
    margin-right: 16px;
  }
  button {
    width: 32px;
    height: 32px;
    /* border-radius: 50%; */
    background-color: #fff;
    outline: none;
    border: none;
  }
`;
const SectionRow = styled.div`
  width: calc(100% - 48px);
  display: flex;
  flex-wrap: wrap;
  padding: 16px;
  border: solid 0.5px ${(props) => props.theme.color.ui.low};
  border-radius: 4px;
  margin-bottom: 24px;
`;

const CheckContainer = styled.div`
  width: 20%;
  height: 32px;
  display: flex;
  align-items: center;
  /* justify-content: space-between; */
  label {
  }
  input {
    margin-right: 12px;
  }
  @media only screen and (max-width: 480px) {
    width: 33%;
  }
`;

const TableSectioProps = {
  topHeight: String,
  bottomHeight: String,
};

const TableSection = styled("div", TableSectioProps)`
  width: 100%;
  height: ${(props) =>
    `calc(100% - ${props.topHeight} - ${props.bottomHeight} - 96px - 24px - 24px)`};
  overflow-y: auto;
  table {
    margin-bottom: 48px;
  }
  td,
  th,
  tr {
    padding: 0px !important;
    text-align: center !important;
  }
  @media only screen and (max-width: 480px) {
    height: 480px;
  }
`;

export default {
  name: "BusRouteSlide",
  components: {
    Container,
    Section,
    MeasureTime,
    SectionRow,
    TableSection,
    KeyIndicatorTable,
    LegendIndicator,
    CheckContainer,
  },
  mounted() {
    console.log(this.$refs.top.$el.clientHeight);
    this.topHeight = this.$refs.top.$el.clientHeight + "px";
    this.bottomHeight = this.$refs.bottom.$el.clientHeight + "px";
  },
  data() {
    return {
      CODE,
      SCALE,
      TYPE,
      ic_refrsh,
      checkedNames: ["모두 선택"],
      measureTime: "2021-10-21  13:11:29",
      topHeight: null,
      bottomHeight: null,
      busRouteList: [
        {
          number: "모두 선택",
          isCheck: true,
        },
        {
          number: "240번",
          isCheck: false,
        },
        {
          number: "425번",
          isCheck: false,
        },
        {
          number: "503번",
          isCheck: false,
        },
        {
          number: "523번",
          isCheck: false,
        },
        {
          number: "623번",
          isCheck: false,
        },
        {
          number: "724번",
          isCheck: false,
        },
        {
          number: "730번",
          isCheck: false,
        },
        {
          number: "805번",
          isCheck: false,
        },
        {
          number: "937번",
          isCheck: false,
        },
      ],
      routeDataDummyList: {
        header: [
          "노선\n(차량번호) ",
          "초미세먼지\n(㎍/㎥)",
          "이산화질소\n(ppm)",
          "오존\n(ppm)",
          "온도\n(°C)",
          "습도\n(%)",
        ],
        data: [
          {
            busNumber: "5070",
            data: [
              { type: TYPE.DUST, value: 23.0 },
              { type: TYPE.NO2, value: 0.012 },
              { type: TYPE.O3, value: 0.0031 },
              { type: TYPE.TEMPERATURE, value: 35.6 },
              { type: TYPE.HUMID, value: 65.0 },
            ],
          },
          {
            busNumber: "5070",
            data: [
              { type: TYPE.DUST, value: 15.0 },
              { type: TYPE.NO2, value: 0.003 },
              { type: TYPE.O3, value: 0.0001 },
              { type: TYPE.TEMPERATURE, value: 35.2 },
              { type: TYPE.HUMID, value: 65.0 },
            ],
          },
          {
            busNumber: "5070",
            data: [
              { type: TYPE.DUST, value: 13.0 },
              { type: TYPE.NO2, value: 0.009 },
              { type: TYPE.O3, value: 0.0023 },
              { type: TYPE.TEMPERATURE, value: 35.0 },
              { type: TYPE.HUMID, value: 65.0 },
            ],
          },
          {
            busNumber: "5070",
            data: [
              { type: TYPE.DUST, value: 50.0 },
              { type: TYPE.NO2, value: 0.012 },
              { type: TYPE.O3, value: 0.0031 },
              { type: TYPE.TEMPERATURE, value: 29.0 },
              { type: TYPE.HUMID, value: 43.0 },
            ],
          },
          {
            busNumber: "5070",
            data: [
              { type: TYPE.DUST, value: 65.0 },
              { type: TYPE.NO2, value: 0.009 },
              { type: TYPE.O3, value: 0.0031 },
              { type: TYPE.TEMPERATURE, value: 32.0 },
              { type: TYPE.HUMID, value: 12.0 },
            ],
          },
          {
            busNumber: "5070",
            data: [
              { type: TYPE.DUST, value: 25.0 },
              { type: TYPE.NO2, value: 0.019 },
              { type: TYPE.O3, value: 0.0031 },
              { type: TYPE.TEMPERATURE, value: 34.0 },
              { type: TYPE.HUMID, value: 23.0 },
            ],
          },
          {
            busNumber: "5070",
            data: [
              { type: TYPE.DUST, value: 8.0 },
              { type: TYPE.NO2, value: 0.0017 },
              { type: TYPE.O3, value: 0.0031 },
              { type: TYPE.TEMPERATURE, value: 35.6 },
              { type: TYPE.HUMID, value: 89.0 },
            ],
          },
          {
            busNumber: "5070",
            data: [
              { type: TYPE.DUST, value: 45.0 },
              { type: TYPE.NO2, value: 0.011 },
              { type: TYPE.O3, value: 0.0031 },
              { type: TYPE.TEMPERATURE, value: 35.0 },
              { type: TYPE.HUMID, value: 45.0 },
            ],
          },
          {
            busNumber: "5070",
            data: [
              { type: TYPE.DUST, value: 12.0 },
              { type: TYPE.NO2, value: 0.009 },
              { type: TYPE.O3, value: 0.0031 },
              { type: TYPE.TEMPERATURE, value: 35.0 },
              { type: TYPE.HUMID, value: 12.0 },
            ],
          },
        ],
      },
    };
  },
  props: {
    isPop: Boolean,
    togglePop: Function,
  },
  methods: {
    levelValidator(value, type) {
      if (type === TYPE.DUST || type === TYPE.NO2 || type === TYPE.O3) {
        if (value < SCALE[type][1]) return CODE.GOOD;
        else if (value < SCALE[type][2]) return CODE.NORMAL;
        else if (value < SCALE[type][3]) return CODE.BAD;
        else return CODE.WORSE;
      } else {
        return CODE.NONE;
      }
    },
  },
};
</script>
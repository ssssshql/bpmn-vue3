<script setup>
import { ref,useTemplateRef } from 'vue'
import BpmnModeler from 'bpmn-js/lib/Modeler';
import { 
  BpmnPropertiesPanelModule, 
  BpmnPropertiesProviderModule,
} from 'bpmn-js-properties-panel';
import { data2 } from './data.js';

const canvasRef = useTemplateRef('canvasRef');
const propertiesRef = useTemplateRef('propertiesRef');
const bpmnModeler = ref(null);

function init(){
  bpmnModeler.value = new BpmnModeler({
    container: canvasRef.value,
    propertiesPanel: {
      parent: propertiesRef.value
    },
    additionalModules: [
      BpmnPropertiesPanelModule,
      BpmnPropertiesProviderModule,
    ],
  });
  bpmnModeler.value.importXML(data2)
    .then(() => {
      console.log('BPMN diagram loaded successfully');
    })
    .catch(err => {
      console.error('Failed to load BPMN diagram:', err);
    });
}

</script>

<template>
  <button @click="init">Load BPMN Diagram</button>
  <div class="canvas" id="js-canvas" ref="canvasRef"></div>
  <div class="properties-panel-parent" id="js-properties-panel" ref="propertiesRef"></div>
</template>

<style scoped>
#js-canvas {
  width: 100%;
  height: 100%;
}
#js-properties-panel {
  position: absolute;
  top: 0;
  border-left: 1px solid #ccc;
  right: 0;
  width: 300px;
  height: 100%;
}
</style>

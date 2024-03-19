<template>
    <div class="priority-tags">
      <div v-for="(priority, index) in priorityList" :key="index" class="priority-tag">
        <div class="circle" :class="priority.class"></div>
        <span class="tag-name">{{ priority.name }}</span>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'PriorityIndicator',
    data() {
      return {
        priorityList: [
          { name: 'High', class: 'high-priority' },
          { name: 'Medium', class: 'medium-priority' },
          { name: 'Low', class: 'low-priority' }
        ]
      };
    }
  };
  </script>
  
  <style scoped>
  .priority-tags {
    display: flex;
    justify-content: space-between;
    align-items: center;
    width: 25%;
    margin: 0 auto;
  }
  
  .priority-tag {
    display: flex;
    align-items: center;
    padding: 1rem 2rem;
    border:1px solid  #f8f9fa;
    border-radius: 25px;
    gap: 10px;
  }
  
  .circle {
    height: 20px;
    width: 20px;
    border-radius: 50%;
    margin-right: 5px;
  }
  
  .tag-name {
    font-size: 14px;
    color: #fff;
  }
  
  /* Priority tag colors */
  .high-priority {
    border: 2px solid red;
  }
  
  .medium-priority {
    border: 2px solid green;
  }
  
  .low-priority {
    border: 2px solid yellow;
  }
  </style>
  
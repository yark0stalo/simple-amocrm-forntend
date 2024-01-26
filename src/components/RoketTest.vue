<template>
  <a-table :columns="columns" :data-source="data">
    <template #expandedRowRender="{ record }">
      <p style="margin: 0">
        {{ record.description }}
      </p>
    </template>
    <template #expandColumnTitle>
      <span style="color: red">More</span>
    </template>
  </a-table>
</template>
<script lang="ts" setup>
let leadsInfo: any;
await fetch("http://localhost:3000/leads", { method: "GET" })
  .then((res) => res.json())
  .then((data) => {
    leadsInfo = data;
  });
const columns = [
  { title: "Название", dataIndex: "name", key: "name", fixed: true },
  { title: "Бюджет", dataIndex: "price", key: "price" },
  { title: "Статус", dataIndex: "status", key: "status" },
  { title: "Ответственный", dataIndex: "responsible", key: "responsible" },
  { title: "Дата создания", dataIndex: "date_creation", key: "date_creation" },
];
const data: any[] = [];
leadsInfo.forEach((lead: any) => {
  let newData = {
    key: lead.id,
    name: lead.name,
    price: lead.price,
    status: lead.status.name,
    responsible: lead.responsible_user,
    date_creation: lead.created_at,
    description:
      "Контакт: " +
      lead.contacts[0].name +
      " Телефон: " +
      lead.contacts[0].phone_number +
      " Email: " +
      lead.contacts[0].email,
  };
  data.push(newData);
});
</script>

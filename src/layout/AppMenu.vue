<script setup>
import { ref, computed } from 'vue';
import AppMenuItem from './AppMenuItem.vue';

// Obtener la información del usuario desde localStorage
const user = JSON.parse(localStorage.getItem('user'));

const model = ref([
{
    label: 'Inicio',
    items: [
        {label: 'Dashboard', icon: 'pi pi-fw pi-home', to: '/dashboard' },
        {label: 'Gestion Humana', icon: 'pi pi-fw pi-users', to:'/rrhh'},
        {label: 'Operadores', icon: 'pi pi-fw pi-phone', to:'/operadores'},
        {label: 'Uri', icon: 'pi pi-fw pi-globe', to:'/uri'},
        {label: 'Tecnologia', icon: 'pi pi-fw pi-globe', to:'/tecnologia'},
        {label: 'Administrar', icon: 'pi pi-fw pi-globe', to:'/admin'},
        {label: 'Articulacion', icon: 'pi pi-fw pi-globe', to:'/articulacion'},
    ]
  },
]);

// Filtrar elementos del menú basados en los roles del usuario
const filteredModel = computed(() => {
    if (user && user.roles.includes("0")) {
        return model.value; // Administrador, acceso a todos los módulos
    }
    return model.value.map(section => {
        return {
            ...section,
            items: section.items.filter(item => {
                // Definir roles permitidos para cada ruta
                const routeRoles = {
                    '/rrhh': [1, 0],
                    '/operadores': [2, 0],
                    '/uri': [3, 0],
                    '/tecnologia': [4, 0],
                    '/articulacion': [8, 0],
                    '/dashboard': [1, 2, 3, 4, 0],
                    '/admin': [0]
                };
                return routeRoles[item.to] && routeRoles[item.to].some(role => user.roles.includes(role.toString()));
            })
        };
    });
});

/*
{
    label: 'UI Components',
    items: [
    { label: 'Form Layout', icon: 'pi pi-fw pi-id-card', to: '/uikit/formlayout' },
    { label: 'Input', icon: 'pi pi-fw pi-check-square', to: '/uikit/input' },
    { label: 'Float Label', icon: 'pi pi-fw pi-bookmark', to: '/uikit/floatlabel' },
    { label: 'Invalid State', icon: 'pi pi-fw pi-exclamation-circle', to: '/uikit/invalidstate' },
    { label: 'Button', icon: 'pi pi-fw pi-mobile', to: '/uikit/button', class: 'rotated-icon' },
    { label: 'Table', icon: 'pi pi-fw pi-table', to: '/uikit/table' },
    { label: 'List', icon: 'pi pi-fw pi-list', to: '/uikit/list' },
    { label: 'Tree', icon: 'pi pi-fw pi-share-alt', to: '/uikit/tree' },
    { label: 'Panel', icon: 'pi pi-fw pi-tablet', to: '/uikit/panel' },
    { label: 'Overlay', icon: 'pi pi-fw pi-clone', to: '/uikit/overlay' },
    { label: 'Media', icon: 'pi pi-fw pi-image', to: '/uikit/media' },
    { label: 'Menu', icon: 'pi pi-fw pi-bars', to: '/uikit/menu', preventExact: true },
    { label: 'Message', icon: 'pi pi-fw pi-comment', to: '/uikit/message' },
    { label: 'File', icon: 'pi pi-fw pi-file', to: '/uikit/file' },
    { label: 'Chart', icon: 'pi pi-fw pi-chart-bar', to: '/uikit/charts' },
    { label: 'Misc', icon: 'pi pi-fw pi-circle', to: '/uikit/misc' }
    ]
},
{
    label: 'Prime Blocks',
    items: [
    { label: 'Free Blocks', icon: 'pi pi-fw pi-eye', to: '/blocks', badge: 'NEW' },
    { label: 'All Blocks', icon: 'pi pi-fw pi-globe', url: 'https://www.primefaces.org/primeblocks-vue', target: '_blank' }
    ]
},
{
    label: 'Utilities',
    items: [
    { label: 'PrimeIcons', icon: 'pi pi-fw pi-prime', to: '/utilities/icons' },
    { label: 'PrimeFlex', icon: 'pi pi-fw pi-desktop', url: 'https://www.primefaces.org/primeflex/', target: '_blank' }
    ]
},
{
    label: 'Pages',
    icon: 'pi pi-fw pi-briefcase',
    to: '/pages',
    items: [
    {
        label: 'Landing',
        icon: 'pi pi-fw pi-globe',
        to: '/landing'
    },
    {
        label: 'Auth',
        icon: 'pi pi-fw pi-user',
        items: [
        {
            label: 'Login',
            icon: 'pi pi-fw pi-sign-in',
            to: '/auth/login'
        },
        {
            label: 'Error',
            icon: 'pi pi-fw pi-times-circle',
            to: '/auth/error'
        },
        {
            label: 'Access Denied',
            icon: 'pi pi-fw pi-lock',
            to: '/auth/access'
        }
        ]
    },
    {
        label: 'Crud',
        icon: 'pi pi-fw pi-pencil',
        to: '/pages/crud'
    },
    {
        label: 'Timeline',
        icon: 'pi pi-fw pi-calendar',
        to: '/pages/timeline'
    },
    {
        label: 'Not Found',
        icon: 'pi pi-fw pi-exclamation-circle',
        to: '/pages/notfound'
    },
    {
        label: 'Empty',
        icon: 'pi pi-fw pi-circle-off',
        to: '/pages/empty'
    }
    ]
},
{
    label: 'Hierarchy',
    items: [
    {
        label: 'Submenu 1',
        icon: 'pi pi-fw pi-bookmark',
        items: [
        {
            label: 'Submenu 1.1',
            icon: 'pi pi-fw pi-bookmark',
            items: [
            { label: 'Submenu 1.1.1', icon: 'pi pi-fw pi-bookmark' },
            { label: 'Submenu 1.1.2', icon: 'pi pi-fw pi-bookmark' },
            { label: 'Submenu 1.1.3', icon: 'pi pi-fw pi-bookmark' }
            ]
        },
        {
            label: 'Submenu 1.2',
            icon: 'pi pi-fw pi-bookmark',
            items: [{ label: 'Submenu 1.2.1', icon: 'pi pi-fw pi-bookmark' }]
        }
        ]
    },
    {
        label: 'Submenu 2',
        icon: 'pi pi-fw pi-bookmark',
        items: [
        {
            label: 'Submenu 2.1',
            icon: 'pi pi-fw pi-bookmark',
            items: [
            { label: 'Submenu 2.1.1', icon: 'pi pi-fw pi-bookmark' },
            { label: 'Submenu 2.1.2', icon: 'pi pi-fw pi-bookmark' }
            ]
        },
        {
            label: 'Submenu 2.2',
            icon: 'pi pi-fw pi-bookmark',
            items: [{ label: 'Submenu 2.2.1', icon: 'pi pi-fw pi-bookmark' }]
        }
        ]
    }
    ]
},
{
    label: 'Get Started',
    items: [
    {
        label: 'Documentation',
        icon: 'pi pi-fw pi-question',
        to: '/documentation'
    },
    {
        label: 'View Source',
        icon: 'pi pi-fw pi-search',
        url: 'https://github.com/primefaces/sakai-vue',
        target: '_blank'
    },
    {
        label: 'Nuxt Version',
        url: 'https://github.com/primefaces/sakai-nuxt',
        icon: 'pi pi-fw pi-star'
    }
    ]
}
*/
</script>

<template>
    <ul class="layout-menu">
        <template v-for="(item, i) in filteredModel" :key="i">
            <app-menu-item v-if="!item.separator" :item="item" :index="i"></app-menu-item>
            <li v-if="item.separator" class="menu-separator"></li>
        </template>
    </ul>
</template>

<style lang="scss" scoped></style>
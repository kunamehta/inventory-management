<template>
  <div class="app" :class="{ 'sidebar-collapsed': sidebarCollapsed }">
    <!-- Left Sidebar -->
    <div v-if="isMobile && !sidebarCollapsed" class="sidebar-backdrop" @click="sidebarCollapsed = true"></div>
    <aside class="sidebar" :class="{ collapsed: sidebarCollapsed, 'mobile-open': isMobile && !sidebarCollapsed }">
      <!-- Sidebar Header -->
      <div class="sidebar-header">
        <div class="sidebar-logo">
          <div class="logo-icon">CC</div>
          <span class="logo-name" v-if="!sidebarCollapsed">{{ t('nav.companyName') }}</span>
        </div>
        <button class="collapse-btn" @click="toggleSidebar" :title="sidebarCollapsed ? 'Expand sidebar' : 'Collapse sidebar'">
          <svg width="16" height="16" viewBox="0 0 16 16" fill="none">
            <path v-if="!sidebarCollapsed" d="M10 4L6 8L10 12" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
            <path v-else d="M6 4L10 8L6 12" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
        </button>
      </div>

      <!-- Navigation -->
      <nav class="sidebar-nav">
        <div class="nav-section">
          <router-link to="/" class="nav-item" :class="{ active: $route.path === '/' }" :title="sidebarCollapsed ? t('nav.overview') : ''">
            <span class="nav-icon">
              <svg width="16" height="16" viewBox="0 0 16 16" fill="none">
                <rect x="1" y="1" width="6" height="6" rx="1.5" stroke="currentColor" stroke-width="1.5"/>
                <rect x="9" y="1" width="6" height="6" rx="1.5" stroke="currentColor" stroke-width="1.5"/>
                <rect x="1" y="9" width="6" height="6" rx="1.5" stroke="currentColor" stroke-width="1.5"/>
                <rect x="9" y="9" width="6" height="6" rx="1.5" stroke="currentColor" stroke-width="1.5"/>
              </svg>
            </span>
            <span class="nav-label" v-if="!sidebarCollapsed">{{ t('nav.overview') }}</span>
          </router-link>

          <router-link to="/inventory" class="nav-item" :class="{ active: $route.path === '/inventory' }" :title="sidebarCollapsed ? t('nav.inventory') : ''">
            <span class="nav-icon">
              <svg width="16" height="16" viewBox="0 0 16 16" fill="none">
                <rect x="1" y="3" width="14" height="10" rx="1.5" stroke="currentColor" stroke-width="1.5"/>
                <path d="M1 6H15" stroke="currentColor" stroke-width="1.5"/>
                <path d="M5 3V6" stroke="currentColor" stroke-width="1.5"/>
                <path d="M11 3V6" stroke="currentColor" stroke-width="1.5"/>
              </svg>
            </span>
            <span class="nav-label" v-if="!sidebarCollapsed">{{ t('nav.inventory') }}</span>
          </router-link>

          <router-link to="/orders" class="nav-item" :class="{ active: $route.path === '/orders' }" :title="sidebarCollapsed ? t('nav.orders') : ''">
            <span class="nav-icon">
              <svg width="16" height="16" viewBox="0 0 16 16" fill="none">
                <path d="M2 2H3.5L5.5 10H12.5L14 5H5" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
                <circle cx="6.5" cy="13" r="1" fill="currentColor"/>
                <circle cx="11.5" cy="13" r="1" fill="currentColor"/>
              </svg>
            </span>
            <span class="nav-label" v-if="!sidebarCollapsed">{{ t('nav.orders') }}</span>
          </router-link>

          <router-link to="/spending" class="nav-item" :class="{ active: $route.path === '/spending' }" :title="sidebarCollapsed ? t('nav.finance') : ''">
            <span class="nav-icon">
              <svg width="16" height="16" viewBox="0 0 16 16" fill="none">
                <circle cx="8" cy="8" r="6.5" stroke="currentColor" stroke-width="1.5"/>
                <path d="M8 4.5V5.5M8 10.5V11.5M6 7C6 6.17 6.895 5.5 8 5.5C9.105 5.5 10 6.17 10 7C10 7.83 9.105 8.5 8 8.5C6.895 8.5 6 9.17 6 10C6 10.83 6.895 11.5 8 11.5C9.105 11.5 10 10.83 10 10" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
              </svg>
            </span>
            <span class="nav-label" v-if="!sidebarCollapsed">{{ t('nav.finance') }}</span>
          </router-link>

          <router-link to="/demand" class="nav-item" :class="{ active: $route.path === '/demand' }" :title="sidebarCollapsed ? t('nav.demandForecast') : ''">
            <span class="nav-icon">
              <svg width="16" height="16" viewBox="0 0 16 16" fill="none">
                <path d="M1.5 12L5.5 7L8.5 9.5L12 5L14.5 7" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
                <path d="M12 4H15V7" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
              </svg>
            </span>
            <span class="nav-label" v-if="!sidebarCollapsed">{{ t('nav.demandForecast') }}</span>
          </router-link>

          <router-link to="/restocking" class="nav-item" :class="{ active: $route.path === '/restocking' }" :title="sidebarCollapsed ? t('nav.restocking') : ''">
            <span class="nav-icon">
              <svg width="16" height="16" viewBox="0 0 16 16" fill="none">
                <path d="M8 2V10M8 2L5 5M8 2L11 5" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
                <path d="M2 11V13C2 13.552 2.448 14 3 14H13C13.552 14 14 13.552 14 13V11" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
              </svg>
            </span>
            <span class="nav-label" v-if="!sidebarCollapsed">{{ t('nav.restocking') }}</span>
          </router-link>

          <router-link to="/reports" class="nav-item" :class="{ active: $route.path === '/reports' }" :title="sidebarCollapsed ? 'Reports' : ''">
            <span class="nav-icon">
              <svg width="16" height="16" viewBox="0 0 16 16" fill="none">
                <rect x="2" y="1.5" width="12" height="13" rx="1.5" stroke="currentColor" stroke-width="1.5"/>
                <path d="M5 5.5H11M5 8H11M5 10.5H8.5" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
              </svg>
            </span>
            <span class="nav-label" v-if="!sidebarCollapsed">Reports</span>
          </router-link>
        </div>
      </nav>

      <!-- Sidebar Footer -->
      <div class="sidebar-footer">
        <LanguageSwitcher />
        <ProfileMenu
          @show-profile-details="showProfileDetails = true"
          @show-tasks="showTasks = true"
        />
      </div>
    </aside>

    <!-- Main Content Area -->
    <div class="main-wrapper">
      <!-- Top Bar with Filters -->
      <header class="top-bar">
        <button v-if="isMobile" class="hamburger-btn" @click="sidebarCollapsed = false" title="Open menu">
          <svg width="20" height="20" viewBox="0 0 20 20" fill="none">
            <path d="M3 5H17M3 10H17M3 15H17" stroke="currentColor" stroke-width="1.75" stroke-linecap="round"/>
          </svg>
        </button>
        <FilterBar />
      </header>

      <!-- Page Content -->
      <main class="main-content">
        <router-view />
      </main>
    </div>

    <ProfileDetailsModal
      :is-open="showProfileDetails"
      @close="showProfileDetails = false"
    />

    <TasksModal
      :is-open="showTasks"
      :tasks="tasks"
      @close="showTasks = false"
      @add-task="addTask"
      @delete-task="deleteTask"
      @toggle-task="toggleTask"
    />
  </div>
</template>

<script>
import { ref, onMounted, onUnmounted, computed } from 'vue'
import { api } from './api'
import { useAuth } from './composables/useAuth'
import { useI18n } from './composables/useI18n'
import FilterBar from './components/FilterBar.vue'
import ProfileMenu from './components/ProfileMenu.vue'
import ProfileDetailsModal from './components/ProfileDetailsModal.vue'
import TasksModal from './components/TasksModal.vue'
import LanguageSwitcher from './components/LanguageSwitcher.vue'

export default {
  name: 'App',
  components: {
    FilterBar,
    ProfileMenu,
    ProfileDetailsModal,
    TasksModal,
    LanguageSwitcher
  },
  setup() {
    const { currentUser } = useAuth()
    const { t } = useI18n()
    const showProfileDetails = ref(false)
    const showTasks = ref(false)
    const apiTasks = ref([])
    const sidebarCollapsed = ref(false)

    const toggleSidebar = () => {
      sidebarCollapsed.value = !sidebarCollapsed.value
    }

    const autoCollapsed = ref(false)
    const isMobile = ref(false)

    const handleResize = () => {
      const w = window.innerWidth
      isMobile.value = w <= 768

      if (w <= 1024 && !sidebarCollapsed.value) {
        autoCollapsed.value = true
        sidebarCollapsed.value = true
      } else if (w > 1024 && autoCollapsed.value) {
        autoCollapsed.value = false
        sidebarCollapsed.value = false
      }
    }

    // Merge mock tasks from currentUser with API tasks
    const tasks = computed(() => {
      return [...currentUser.value.tasks, ...apiTasks.value]
    })

    const loadTasks = async () => {
      try {
        apiTasks.value = await api.getTasks()
      } catch (err) {
        console.error('Failed to load tasks:', err)
      }
    }

    const addTask = async (taskData) => {
      try {
        const newTask = await api.createTask(taskData)
        // Add new task to the beginning of the array
        apiTasks.value.unshift(newTask)
      } catch (err) {
        console.error('Failed to add task:', err)
      }
    }

    const deleteTask = async (taskId) => {
      try {
        // Check if it's a mock task (from currentUser)
        const isMockTask = currentUser.value.tasks.some(t => t.id === taskId)

        if (isMockTask) {
          // Remove from mock tasks
          const index = currentUser.value.tasks.findIndex(t => t.id === taskId)
          if (index !== -1) {
            currentUser.value.tasks.splice(index, 1)
          }
        } else {
          // Remove from API tasks
          await api.deleteTask(taskId)
          apiTasks.value = apiTasks.value.filter(t => t.id !== taskId)
        }
      } catch (err) {
        console.error('Failed to delete task:', err)
      }
    }

    const toggleTask = async (taskId) => {
      try {
        // Check if it's a mock task (from currentUser)
        const mockTask = currentUser.value.tasks.find(t => t.id === taskId)

        if (mockTask) {
          // Toggle mock task status
          mockTask.status = mockTask.status === 'pending' ? 'completed' : 'pending'
        } else {
          // Toggle API task
          const updatedTask = await api.toggleTask(taskId)
          const index = apiTasks.value.findIndex(t => t.id === taskId)
          if (index !== -1) {
            apiTasks.value[index] = updatedTask
          }
        }
      } catch (err) {
        console.error('Failed to toggle task:', err)
      }
    }

    onMounted(() => {
      loadTasks()
      handleResize()
      window.addEventListener('resize', handleResize)
    })

    onUnmounted(() => {
      window.removeEventListener('resize', handleResize)
    })

    return {
      t,
      showProfileDetails,
      showTasks,
      tasks,
      addTask,
      deleteTask,
      toggleTask,
      sidebarCollapsed,
      toggleSidebar,
      isMobile
    }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
  background: #f8fafc;
  color: #1e293b;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

/* ─── App Shell ─────────────────────────────────────────── */
.app {
  display: flex;
  min-height: 100vh;
  background: #f8fafc;
}

/* ─── Sidebar ────────────────────────────────────────────── */
.sidebar {
  width: 220px;
  min-width: 220px;
  background: #0f172a;
  border-right: 1px solid #1e293b;
  display: flex;
  flex-direction: column;
  position: fixed;
  top: 0;
  left: 0;
  bottom: 0;
  z-index: 100;
  transition: width 0.25s ease, min-width 0.25s ease;
  overflow: hidden;
}

.sidebar.collapsed {
  width: 64px;
  min-width: 64px;
}

/* ─── Sidebar Header ─────────────────────────────────────── */
.sidebar-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 14px;
  height: 56px;
  border-bottom: 1px solid #1e293b;
  flex-shrink: 0;
}

.sidebar.collapsed .sidebar-header {
  justify-content: center;
  padding: 0;
}

.sidebar-logo {
  display: flex;
  align-items: center;
  gap: 10px;
  overflow: hidden;
  min-width: 0;
}

.logo-icon {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
  width: 28px;
  height: 28px;
  background: #2563eb;
  border-radius: 6px;
  font-size: 0.75rem;
  font-weight: 700;
  color: #ffffff;
  letter-spacing: 0;
}

.logo-name {
  font-size: 0.875rem;
  font-weight: 600;
  color: #ffffff;
  letter-spacing: -0.01em;
  white-space: nowrap;
  overflow: hidden;
}

.collapse-btn {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 24px;
  height: 24px;
  background: transparent;
  border: none;
  border-radius: 5px;
  color: #64748b;
  cursor: pointer;
  flex-shrink: 0;
  transition: all 0.15s ease;
}

.collapse-btn:hover {
  background: rgba(255, 255, 255, 0.06);
  color: #94a3b8;
}

.sidebar.collapsed .collapse-btn {
  display: none;
}

/* ─── Sidebar Nav ────────────────────────────────────────── */
.sidebar-nav {
  flex: 1;
  padding: 8px 0;
  overflow-y: auto;
  overflow-x: hidden;
}

.sidebar-nav::-webkit-scrollbar {
  width: 3px;
}
.sidebar-nav::-webkit-scrollbar-track {
  background: transparent;
}
.sidebar-nav::-webkit-scrollbar-thumb {
  background: #1e293b;
  border-radius: 2px;
}

.nav-section {
  padding: 0 8px;
}

.nav-item {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 8px 10px;
  border-radius: 6px;
  border-left: 3px solid transparent;
  color: #94a3b8;
  text-decoration: none;
  font-size: 0.8125rem;
  font-weight: 500;
  transition: all 0.12s ease;
  white-space: nowrap;
  margin-bottom: 2px;
}

.nav-item:hover {
  background: rgba(255, 255, 255, 0.05);
  color: #cbd5e1;
}

.nav-item.active {
  background: rgba(37, 99, 235, 0.15);
  border-left-color: #2563eb;
  color: #f1f5f9;
}

.nav-item.active .nav-icon {
  color: #f1f5f9;
}

.nav-icon {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
  width: 16px;
  height: 16px;
}

.nav-label {
  overflow: hidden;
  text-overflow: ellipsis;
}

.sidebar.collapsed .nav-item {
  padding: 10px;
  justify-content: center;
  border-left-color: transparent;
}

.sidebar.collapsed .nav-item.active {
  background: rgba(37, 99, 235, 0.15);
}

/* ─── Sidebar Footer ─────────────────────────────────────── */
.sidebar-footer {
  border-top: 1px solid #1e293b;
  padding: 8px;
  display: flex;
  flex-direction: column;
  gap: 2px;
}

/* ─── Main Wrapper ───────────────────────────────────────── */
.main-wrapper {
  flex: 1;
  display: flex;
  flex-direction: column;
  min-height: 100vh;
  margin-left: 220px;
  transition: margin-left 0.25s ease;
  background: #f8fafc;
}

.app.sidebar-collapsed .main-wrapper {
  margin-left: 64px;
}

/* ─── Top Bar (filters) ──────────────────────────────────── */
.top-bar {
  background: #ffffff;
  border-bottom: 1px solid #e2e8f0;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.04);
  position: sticky;
  top: 0;
  z-index: 90;
  display: flex;
  align-items: center;
}

/* ─── Main Content ───────────────────────────────────────── */
.main-content {
  flex: 1;
  padding: 1.5rem 2rem;
}

/* ─── Global Shared Styles ───────────────────────────────── */
.page-header {
  margin-bottom: 2rem;
}

.page-header h2 {
  font-size: 1.875rem;
  font-weight: 700;
  color: #0f172a;
  margin-bottom: 0.375rem;
  letter-spacing: -0.025em;
}

.page-header p {
  color: #64748b;
  font-size: 0.938rem;
}

.stats-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 1.5rem;
  margin-bottom: 1.5rem;
}

.stat-card {
  background: white;
  padding: 1.25rem;
  border-radius: 10px;
  border: 1px solid #e2e8f0;
  transition: all 0.2s ease;
}

.stat-card:hover {
  border-color: #cbd5e1;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.06);
}

.stat-label {
  color: #64748b;
  font-size: 0.875rem;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  margin-bottom: 0.625rem;
}

.stat-value {
  font-size: 2.25rem;
  font-weight: 700;
  color: #0f172a;
  letter-spacing: -0.025em;
}

.stat-card.warning .stat-value {
  color: #ea580c;
}

.stat-card.success .stat-value {
  color: #059669;
}

.stat-card.danger .stat-value {
  color: #dc2626;
}

.stat-card.info .stat-value {
  color: #2563eb;
}

.card {
  background: white;
  border-radius: 10px;
  padding: 1.25rem;
  border: 1px solid #e2e8f0;
  margin-bottom: 1.5rem;
}

.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1rem;
  padding-bottom: 0.875rem;
  border-bottom: 1px solid #e2e8f0;
}

.card-title {
  font-size: 1.125rem;
  font-weight: 700;
  color: #0f172a;
  letter-spacing: -0.025em;
}

.table-container {
  overflow-x: auto;
}

table {
  width: 100%;
  border-collapse: collapse;
}

thead {
  background: #f8fafc;
  border-top: 1px solid #e2e8f0;
  border-bottom: 1px solid #e2e8f0;
}

th {
  text-align: left;
  padding: 0.5rem 0.75rem;
  font-weight: 600;
  color: #475569;
  font-size: 0.75rem;
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

td {
  padding: 0.5rem 0.75rem;
  border-top: 1px solid #f1f5f9;
  color: #334155;
  font-size: 0.875rem;
}

tbody tr {
  transition: background-color 0.15s ease;
}

tbody tr:hover {
  background: #f8fafc;
}

.badge {
  display: inline-block;
  padding: 0.313rem 0.75rem;
  border-radius: 6px;
  font-size: 0.75rem;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.025em;
}

.badge.success {
  background: #d1fae5;
  color: #065f46;
}

.badge.warning {
  background: #fed7aa;
  color: #92400e;
}

.badge.danger {
  background: #fecaca;
  color: #991b1b;
}

.badge.info {
  background: #dbeafe;
  color: #1e40af;
}

.badge.increasing {
  background: #d1fae5;
  color: #065f46;
}

.badge.decreasing {
  background: #fecaca;
  color: #991b1b;
}

.badge.stable {
  background: #e0e7ff;
  color: #3730a3;
}

.badge.high {
  background: #fecaca;
  color: #991b1b;
}

.badge.medium {
  background: #fed7aa;
  color: #92400e;
}

.badge.low {
  background: #dbeafe;
  color: #1e40af;
}

.loading {
  text-align: center;
  padding: 3rem;
  color: #64748b;
  font-size: 0.938rem;
}

.error {
  background: #fef2f2;
  border: 1px solid #fecaca;
  color: #991b1b;
  padding: 1rem;
  border-radius: 8px;
  margin: 1rem 0;
  font-size: 0.938rem;
}

/* ─── Responsive Sidebar ─────────────────────────────────── */

/* Backdrop for mobile overlay */
.sidebar-backdrop {
  display: none;
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.5);
  z-index: 99;
}

/* Hamburger: hidden by default, shown only on mobile via media query */
.hamburger-btn {
  display: none;
}

/* Mobile: sidebar as overlay, not rail */
@media (max-width: 768px) {
  .sidebar-backdrop {
    display: block;
  }

  .sidebar {
    transform: translateX(-100%);
    transition: transform 0.25s ease, width 0.25s ease;
    width: 220px !important;
    min-width: 220px !important;
  }

  .sidebar.mobile-open {
    transform: translateX(0);
  }

  .main-wrapper {
    margin-left: 0 !important;
  }

  .hamburger-btn {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 36px;
    height: 36px;
    border: none;
    background: transparent;
    cursor: pointer;
    color: #64748b;
    border-radius: 6px;
    margin-left: 0.75rem;
    flex-shrink: 0;
  }

  .hamburger-btn:hover {
    background: #f1f5f9;
  }
}

/* Tablet: icon-rail still pushes content, no hamburger, no backdrop */
@media (min-width: 769px) and (max-width: 1024px) {
  .hamburger-btn {
    display: none;
  }

  .sidebar-backdrop {
    display: none !important;
  }
}

/* Desktop: hamburger never shown */
@media (min-width: 1025px) {
  .hamburger-btn {
    display: none;
  }
}
</style>

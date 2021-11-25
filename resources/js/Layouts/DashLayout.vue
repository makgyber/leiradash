<template>
<Head :title="Dashboard" />
<!-- component -->
<!-- This is an example component -->
<div>
   <nav class="bg-white border-b border-gray-100 fixed w-full z-50">
                <!-- Primary Navigation Menu -->
                <div class=" mx-auto px-4 sm:px-6 lg:px-8">
                    <div class="flex justify-between h-16">
                        <div class="flex">
                            <!-- Logo -->
                            <div class="flex-shrink-0 flex items-center">
                                <Link :href="route('dashboard')">
                                    <jet-application-mark class="block h-9 w-auto" />
                                </Link>
                            </div>

                            <!-- Navigation Links -->
                            <div class="hidden space-x-8 sm:-my-px sm:ml-10 sm:flex">
                                <jet-nav-link :href="route('dashboard')" :active="route().current('dashboard')">
                                    Dashboard
                                </jet-nav-link>
                            </div>
                            <div class="hidden space-x-8 sm:-my-px sm:ml-10 sm:flex">
                                <jet-nav-link :href="route('my-dashboard')" :active="route().current('mydash')">
                                    My Dash
                                </jet-nav-link>
                            </div>
                        </div>

                        <div class="hidden sm:flex sm:items-center sm:ml-6">
                            <div class="ml-3 relative">
                                <!-- Teams Dropdown -->
                                <jet-dropdown align="right" width="60" v-if="$page.props.jetstream.hasTeamFeatures">
                                    <template #trigger>
                                        <span class="inline-flex rounded-md">
                                            <button type="button" class="inline-flex items-center px-3 py-2 border border-transparent text-sm leading-4 font-medium rounded-md text-gray-500 bg-white hover:bg-gray-50 hover:text-gray-700 focus:outline-none focus:bg-gray-50 active:bg-gray-50 transition">
                                                {{ $page.props.user.current_team.name }}

                                                <svg class="ml-2 -mr-0.5 h-4 w-4" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor">
                                                    <path fill-rule="evenodd" d="M10 3a1 1 0 01.707.293l3 3a1 1 0 01-1.414 1.414L10 5.414 7.707 7.707a1 1 0 01-1.414-1.414l3-3A1 1 0 0110 3zm-3.707 9.293a1 1 0 011.414 0L10 14.586l2.293-2.293a1 1 0 011.414 1.414l-3 3a1 1 0 01-1.414 0l-3-3a1 1 0 010-1.414z" clip-rule="evenodd" />
                                                </svg>
                                            </button>
                                        </span>
                                    </template>

                                    <template #content>
                                        <div class="w-60">
                                            <!-- Team Management -->
                                            <template v-if="$page.props.jetstream.hasTeamFeatures">
                                                <div class="block px-4 py-2 text-xs text-gray-400">
                                                    Manage Team
                                                </div>

                                                <!-- Team Settings -->
                                                <jet-dropdown-link :href="route('teams.show', $page.props.user.current_team)">
                                                    Team Settings
                                                </jet-dropdown-link>

                                                <jet-dropdown-link :href="route('teams.create')" v-if="$page.props.jetstream.canCreateTeams">
                                                    Create New Team
                                                </jet-dropdown-link>

                                                <div class="border-t border-gray-100"></div>

                                                <!-- Team Switcher -->
                                                <div class="block px-4 py-2 text-xs text-gray-400">
                                                    Switch Teams
                                                </div>

                                                <template v-for="team in $page.props.user.all_teams" :key="team.id">
                                                    <form @submit.prevent="switchToTeam(team)">
                                                        <jet-dropdown-link as="button">
                                                            <div class="flex items-center">
                                                                <svg v-if="team.id == $page.props.user.current_team_id" class="mr-2 h-5 w-5 text-green-400" fill="none" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" stroke="currentColor" viewBox="0 0 24 24"><path d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z"></path></svg>
                                                                <div>{{ team.name }}</div>
                                                            </div>
                                                        </jet-dropdown-link>
                                                    </form>
                                                </template>
                                            </template>
                                        </div>
                                    </template>
                                </jet-dropdown>
                            </div>

                            <!-- Settings Dropdown -->
                            <div class="ml-3 relative">
                                <jet-dropdown align="right" width="48">
                                    <template #trigger>
                                        <button v-if="$page.props.jetstream.managesProfilePhotos" class="flex text-sm border-2 border-transparent rounded-full focus:outline-none focus:border-gray-300 transition">
                                            <img class="h-8 w-8 rounded-full object-cover" :src="$page.props.user.profile_photo_url" :alt="$page.props.user.name" />
                                        </button>

                                        <span v-else class="inline-flex rounded-md">
                                            <button type="button" class="inline-flex items-center px-3 py-2 border border-transparent text-sm leading-4 font-medium rounded-md text-gray-500 bg-white hover:text-gray-700 focus:outline-none transition">
                                                {{ $page.props.user.name }}

                                                <svg class="ml-2 -mr-0.5 h-4 w-4" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor">
                                                    <path fill-rule="evenodd" d="M5.293 7.293a1 1 0 011.414 0L10 10.586l3.293-3.293a1 1 0 111.414 1.414l-4 4a1 1 0 01-1.414 0l-4-4a1 1 0 010-1.414z" clip-rule="evenodd" />
                                                </svg>
                                            </button>
                                        </span>
                                    </template>

                                    <template #content>
                                        <!-- Account Management -->
                                        <div class="block px-4 py-2 text-xs text-gray-400">
                                            Manage Account
                                        </div>

                                        <jet-dropdown-link :href="route('profile.show')">
                                            Profile
                                        </jet-dropdown-link>

                                        <jet-dropdown-link :href="route('api-tokens.index')" v-if="$page.props.jetstream.hasApiFeatures">
                                            API Tokens
                                        </jet-dropdown-link>

                                        <div class="border-t border-gray-100"></div>

                                        <!-- Authentication -->
                                        <form @submit.prevent="logout">
                                            <jet-dropdown-link as="button">
                                                Log Out
                                            </jet-dropdown-link>
                                        </form>
                                    </template>
                                </jet-dropdown>
                            </div>
                        </div>

                        <!-- Hamburger -->
                        <div class="-mr-2 flex items-center sm:hidden">
                            <button @click="showingNavigationDropdown = ! showingNavigationDropdown" class="inline-flex items-center justify-center p-2 rounded-md text-gray-400 hover:text-gray-500 hover:bg-gray-100 focus:outline-none focus:bg-gray-100 focus:text-gray-500 transition">
                                <svg class="h-6 w-6" stroke="currentColor" fill="none" viewBox="0 0 24 24">
                                    <path :class="{'hidden': showingNavigationDropdown, 'inline-flex': ! showingNavigationDropdown }" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16" />
                                    <path :class="{'hidden': ! showingNavigationDropdown, 'inline-flex': showingNavigationDropdown }" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
                                </svg>
                            </button>
                        </div>
                    </div>
                </div>

                <!-- Responsive Navigation Menu -->
                <div :class="{'block': showingNavigationDropdown, 'hidden': ! showingNavigationDropdown}" class="sm:hidden">
                    <div class="pt-2 pb-3 space-y-1">
                        <jet-responsive-nav-link :href="route('dashboard')" :active="route().current('dashboard')">
                            Dashboard
                        </jet-responsive-nav-link>
                    </div>

                    <!-- Responsive Settings Options -->
                    <div class="pt-4 pb-1 border-t border-gray-200">
                        <div class="flex items-center px-4">
                            <div v-if="$page.props.jetstream.managesProfilePhotos" class="flex-shrink-0 mr-3" >
                                <img class="h-10 w-10 rounded-full object-cover" :src="$page.props.user.profile_photo_url" :alt="$page.props.user.name" />
                            </div>

                            <div>
                                <div class="font-medium text-base text-gray-800">{{ $page.props.user.name }}</div>
                                <div class="font-medium text-sm text-gray-500">{{ $page.props.user.email }}</div>
                            </div>
                        </div>

                        <div class="mt-3 space-y-1">
                            <jet-responsive-nav-link :href="route('profile.show')" :active="route().current('profile.show')">
                                Profile
                            </jet-responsive-nav-link>

                            <jet-responsive-nav-link :href="route('api-tokens.index')" :active="route().current('api-tokens.index')" v-if="$page.props.jetstream.hasApiFeatures">
                                API Tokens
                            </jet-responsive-nav-link>

                            <!-- Authentication -->
                            <form method="POST" @submit.prevent="logout">
                                <jet-responsive-nav-link as="button">
                                    Log Out
                                </jet-responsive-nav-link>
                            </form>

                            <!-- Team Management -->
                            <template v-if="$page.props.jetstream.hasTeamFeatures">
                                <div class="border-t border-gray-200"></div>

                                <div class="block px-4 py-2 text-xs text-gray-400">
                                    Manage Team
                                </div>

                                <!-- Team Settings -->
                                <jet-responsive-nav-link :href="route('teams.show', $page.props.user.current_team)" :active="route().current('teams.show')">
                                    Team Settings
                                </jet-responsive-nav-link>

                                <jet-responsive-nav-link :href="route('teams.create')" :active="route().current('teams.create')" v-if="$page.props.jetstream.canCreateTeams">
                                    Create New Team
                                </jet-responsive-nav-link>

                                <div class="border-t border-gray-200"></div>

                                <!-- Team Switcher -->
                                <div class="block px-4 py-2 text-xs text-gray-400">
                                    Switch Teams
                                </div>

                                <template v-for="team in $page.props.user.all_teams" :key="team.id">
                                    <form @submit.prevent="switchToTeam(team)">
                                        <jet-responsive-nav-link as="button">
                                            <div class="flex items-center">
                                                <svg v-if="team.id == $page.props.user.current_team_id" class="mr-2 h-5 w-5 text-green-400" fill="none" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" stroke="currentColor" viewBox="0 0 24 24"><path d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z"></path></svg>
                                                <div>{{ team.name }}</div>
                                            </div>
                                        </jet-responsive-nav-link>
                                    </form>
                                </template>
                            </template>
                        </div>
                    </div>
                </div>
            </nav>
   <div class="flex overflow-hidden bg-white pt-16">
      <aside id="sidebar" class="fixed hidden z-20 h-full top-0 left-0 pt-16 flex lg:flex flex-shrink-0 flex-col w-64 transition-width duration-75" aria-label="Sidebar">
         <div class="relative flex-1 flex flex-col min-h-0 border-r border-gray-200 bg-white pt-0">
            <div class="flex-1 flex flex-col pt-5 pb-4 overflow-y-auto">
               <div class="flex-1 px-3 bg-white divide-y space-y-1">
                  <ul class="space-y-2 pb-2">
                     <li>
                        <form action="#" method="GET" class="lg:hidden">
                           <label for="mobile-search" class="sr-only">Search</label>
                           <div class="relative">
                              <div class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none">
                                 <svg class="w-5 h-5 text-gray-500" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg">
                                    <path d="M5 3a2 2 0 00-2 2v2a2 2 0 002 2h2a2 2 0 002-2V5a2 2 0 00-2-2H5zM5 11a2 2 0 00-2 2v2a2 2 0 002 2h2a2 2 0 002-2v-2a2 2 0 00-2-2H5zM11 5a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2h-2a2 2 0 01-2-2V5zM11 13a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2h-2a2 2 0 01-2-2v-2z"></path>
                                 </svg>
                              </div>
                              <input type="text" name="email" id="mobile-search" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-cyan-600 focus:ring-cyan-600 block w-full pl-10 p-2.5" placeholder="Search">
                           </div>
                        </form>
                     </li>
                     <li>
                        <a href="https://demo.themesberg.com/windster/" class="text-base text-gray-900 font-normal rounded-lg flex items-center p-2 hover:bg-gray-100 group">
                           <svg class="w-6 h-6 text-gray-500 group-hover:text-gray-900 transition duration-75" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg">
                              <path d="M2 10a8 8 0 018-8v8h8a8 8 0 11-16 0z"></path>
                              <path d="M12 2.252A8.014 8.014 0 0117.748 8H12V2.252z"></path>
                           </svg>
                           <span class="ml-3">Dashboard</span>
                        </a>
                     </li>
                     <li>
                        <a href="https://demo.themesberg.com/windster-pro/kanban/" target="_blank" class="text-base text-gray-900 font-normal rounded-lg hover:bg-gray-100 flex items-center p-2 group ">
                           <svg class="w-6 h-6 text-gray-500 flex-shrink-0 group-hover:text-gray-900 transition duration-75" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg">
                              <path d="M5 3a2 2 0 00-2 2v2a2 2 0 002 2h2a2 2 0 002-2V5a2 2 0 00-2-2H5zM5 11a2 2 0 00-2 2v2a2 2 0 002 2h2a2 2 0 002-2v-2a2 2 0 00-2-2H5zM11 5a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2h-2a2 2 0 01-2-2V5zM11 13a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2h-2a2 2 0 01-2-2v-2z"></path>
                           </svg>
                           <span class="ml-3 flex-1 whitespace-nowrap">Kanban</span>
                           <span class="bg-gray-200 text-gray-800 ml-3 text-sm font-medium inline-flex items-center justify-center px-2 rounded-full">Pro</span>
                        </a>
                     </li>
                     <li>
                        <a href="https://demo.themesberg.com/windster-pro/mailing/inbox/" target="_blank" class="text-base text-gray-900 font-normal rounded-lg hover:bg-gray-100 flex items-center p-2 group ">
                           <svg class="w-6 h-6 text-gray-500 flex-shrink-0 group-hover:text-gray-900 transition duration-75" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg">
                              <path d="M8.707 7.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l2-2a1 1 0 00-1.414-1.414L11 7.586V3a1 1 0 10-2 0v4.586l-.293-.293z"></path>
                              <path d="M3 5a2 2 0 012-2h1a1 1 0 010 2H5v7h2l1 2h4l1-2h2V5h-1a1 1 0 110-2h1a2 2 0 012 2v10a2 2 0 01-2 2H5a2 2 0 01-2-2V5z"></path>
                           </svg>
                           <span class="ml-3 flex-1 whitespace-nowrap">Inbox</span>
                           <span class="bg-gray-200 text-gray-800 ml-3 text-sm font-medium inline-flex items-center justify-center px-2 rounded-full">Pro</span>
                        </a>
                     </li>
                     <li>
                        <a href="https://demo.themesberg.com/windster/users/list/" class="text-base text-gray-900 font-normal rounded-lg hover:bg-gray-100 flex items-center p-2 group ">
                           <svg class="w-6 h-6 text-gray-500 flex-shrink-0 group-hover:text-gray-900 transition duration-75" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg">
                              <path fill-rule="evenodd" d="M10 9a3 3 0 100-6 3 3 0 000 6zm-7 9a7 7 0 1114 0H3z" clip-rule="evenodd"></path>
                           </svg>
                           <span class="ml-3 flex-1 whitespace-nowrap">Users</span>
                        </a>
                     </li>
                     <li>
                        <a href="https://demo.themesberg.com/windster/e-commerce/products/" class="text-base text-gray-900 font-normal rounded-lg hover:bg-gray-100 flex items-center p-2 group ">
                           <svg class="w-6 h-6 text-gray-500 flex-shrink-0 group-hover:text-gray-900 transition duration-75" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg">
                              <path fill-rule="evenodd" d="M10 2a4 4 0 00-4 4v1H5a1 1 0 00-.994.89l-1 9A1 1 0 004 18h12a1 1 0 00.994-1.11l-1-9A1 1 0 0015 7h-1V6a4 4 0 00-4-4zm2 5V6a2 2 0 10-4 0v1h4zm-6 3a1 1 0 112 0 1 1 0 01-2 0zm7-1a1 1 0 100 2 1 1 0 000-2z" clip-rule="evenodd"></path>
                           </svg>
                           <span class="ml-3 flex-1 whitespace-nowrap">Products</span>
                        </a>
                     </li>
                     <li>
                        <a href="https://demo.themesberg.com/windster/authentication/sign-in/" class="text-base text-gray-900 font-normal rounded-lg hover:bg-gray-100 flex items-center p-2 group ">
                           <svg class="w-6 h-6 text-gray-500 flex-shrink-0 group-hover:text-gray-900 transition duration-75" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg">
                              <path fill-rule="evenodd" d="M3 3a1 1 0 00-1 1v12a1 1 0 102 0V4a1 1 0 00-1-1zm10.293 9.293a1 1 0 001.414 1.414l3-3a1 1 0 000-1.414l-3-3a1 1 0 10-1.414 1.414L14.586 9H7a1 1 0 100 2h7.586l-1.293 1.293z" clip-rule="evenodd"></path>
                           </svg>
                           <span class="ml-3 flex-1 whitespace-nowrap">Sign In</span>
                        </a>
                     </li>
                     <li>
                        <a href="https://demo.themesberg.com/windster/authentication/sign-up/" class="text-base text-gray-900 font-normal rounded-lg hover:bg-gray-100 flex items-center p-2 group ">
                           <svg class="w-6 h-6 text-gray-500 flex-shrink-0 group-hover:text-gray-900 transition duration-75" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg">
                              <path fill-rule="evenodd" d="M5 4a3 3 0 00-3 3v6a3 3 0 003 3h10a3 3 0 003-3V7a3 3 0 00-3-3H5zm-1 9v-1h5v2H5a1 1 0 01-1-1zm7 1h4a1 1 0 001-1v-1h-5v2zm0-4h5V8h-5v2zM9 8H4v2h5V8z" clip-rule="evenodd"></path>
                           </svg>
                           <span class="ml-3 flex-1 whitespace-nowrap">Sign Up</span>
                        </a>
                     </li>
                  </ul>
                  <div class="space-y-2 pt-2">
                     <a href="https://demo.themesberg.com/windster/pricing/" class="text-base text-gray-900 font-normal rounded-lg hover:bg-gray-100 group transition duration-75 flex items-center p-2">
                        <svg class="w-5 h-5 text-gray-500 flex-shrink-0 group-hover:text-gray-900 transition duration-75" aria-hidden="true" focusable="false" data-prefix="fas" data-icon="gem" role="img" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512">
                           <path fill="currentColor" d="M378.7 32H133.3L256 182.7L378.7 32zM512 192l-107.4-141.3L289.6 192H512zM107.4 50.67L0 192h222.4L107.4 50.67zM244.3 474.9C247.3 478.2 251.6 480 256 480s8.653-1.828 11.67-5.062L510.6 224H1.365L244.3 474.9z"></path>
                        </svg>
                        <span class="ml-4">Upgrade to Pro</span>
                     </a>
                     <a href="https://flowbite.com/docs/getting-started/introduction/" target="_blank" class="text-base text-gray-900 font-normal rounded-lg hover:bg-gray-100 group transition duration-75 flex items-center p-2">
                        <svg class="w-6 h-6 text-gray-500 flex-shrink-0 group-hover:text-gray-900 transition duration-75" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg">
                           <path d="M9 2a1 1 0 000 2h2a1 1 0 100-2H9z"></path>
                           <path fill-rule="evenodd" d="M4 5a2 2 0 012-2 3 3 0 003 3h2a3 3 0 003-3 2 2 0 012 2v11a2 2 0 01-2 2H6a2 2 0 01-2-2V5zm3 4a1 1 0 000 2h.01a1 1 0 100-2H7zm3 0a1 1 0 000 2h3a1 1 0 100-2h-3zm-3 4a1 1 0 100 2h.01a1 1 0 100-2H7zm3 0a1 1 0 100 2h3a1 1 0 100-2h-3z" clip-rule="evenodd"></path>
                        </svg>
                        <span class="ml-3">Documentation</span>
                     </a>
                     <a href="https://flowbite.com/docs/components/alerts/" target="_blank" class="text-base text-gray-900 font-normal rounded-lg hover:bg-gray-100 group transition duration-75 flex items-center p-2">
                        <svg class="w-6 h-6 text-gray-500 flex-shrink-0 group-hover:text-gray-900 transition duration-75" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg">
                           <path d="M7 3a1 1 0 000 2h6a1 1 0 100-2H7zM4 7a1 1 0 011-1h10a1 1 0 110 2H5a1 1 0 01-1-1zM2 11a2 2 0 012-2h12a2 2 0 012 2v4a2 2 0 01-2 2H4a2 2 0 01-2-2v-4z"></path>
                        </svg>
                        <span class="ml-3">Components</span>
                     </a>
                     <a href="https://github.com/themesberg/windster-tailwind-css-dashboard/issues" target="_blank" class="text-base text-gray-900 font-normal rounded-lg hover:bg-gray-100 group transition duration-75 flex items-center p-2">
                        <svg class="w-6 h-6 text-gray-500 flex-shrink-0 group-hover:text-gray-900 transition duration-75" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg">
                           <path fill-rule="evenodd" d="M18 10a8 8 0 11-16 0 8 8 0 0116 0zm-2 0c0 .993-.241 1.929-.668 2.754l-1.524-1.525a3.997 3.997 0 00.078-2.183l1.562-1.562C15.802 8.249 16 9.1 16 10zm-5.165 3.913l1.58 1.58A5.98 5.98 0 0110 16a5.976 5.976 0 01-2.516-.552l1.562-1.562a4.006 4.006 0 001.789.027zm-4.677-2.796a4.002 4.002 0 01-.041-2.08l-.08.08-1.53-1.533A5.98 5.98 0 004 10c0 .954.223 1.856.619 2.657l1.54-1.54zm1.088-6.45A5.974 5.974 0 0110 4c.954 0 1.856.223 2.657.619l-1.54 1.54a4.002 4.002 0 00-2.346.033L7.246 4.668zM12 10a2 2 0 11-4 0 2 2 0 014 0z" clip-rule="evenodd"></path>
                        </svg>
                        <span class="ml-3">Help</span>
                     </a>
                  </div>
               </div>
            </div>
         </div>
      </aside>
      <div class="bg-gray-900 opacity-50 hidden fixed inset-0 z-10" id="sidebarBackdrop"></div>
      <div id="main-content" class=" min-h-screen h-full w-full bg-gray-50 relative overflow-y-auto lg:ml-64 flex-grow">
          <!-- Page Content -->
            <main>
                <slot></slot>
            </main>
         <p class="text-center text-sm text-gray-500 my-10 fixed-bottom">
            &copy; 2019-2021 <a href="https://themesberg.com" class="hover:underline" target="_blank">Themesberg</a>. All rights reserved.
         </p>
      </div>
   </div>
</div>
</template>
<script>
    import { defineComponent } from 'vue'
    import JetApplicationMark from '@/Jetstream/ApplicationMark.vue'
    import JetBanner from '@/Jetstream/Banner.vue'
    import JetDropdown from '@/Jetstream/Dropdown.vue'
    import JetDropdownLink from '@/Jetstream/DropdownLink.vue'
    import JetNavLink from '@/Jetstream/NavLink.vue'
    import JetResponsiveNavLink from '@/Jetstream/ResponsiveNavLink.vue'
    import { Head, Link } from '@inertiajs/inertia-vue3';

    export default defineComponent({
        props: {
            title: String,
        },

        components: {
            Head,
            JetApplicationMark,
            JetBanner,
            JetDropdown,
            JetDropdownLink,
            JetNavLink,
            JetResponsiveNavLink,
            Link,
        },

        data() {
            return {
                showingNavigationDropdown: false,
            }
        },

        methods: {
            switchToTeam(team) {
                this.$inertia.put(route('current-team.update'), {
                    'team_id': team.id
                }, {
                    preserveState: false
                })
            },

            logout() {
                this.$inertia.post(route('logout'));
            },
        }
    })
</script>

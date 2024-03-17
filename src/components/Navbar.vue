<template>
    <nav class="navbar">
        <span class="logo logo_primary">{{primaryName}} <span class="logo logo_second">{{ secondaryName }}</span></span>
        <a class="logout" href=""><i class="fa-solid fa-right-from-bracket"></i></a>
    </nav>
</template>

<script>
export default {
    name: 'NavBar',
    data() {
        return {
            primaryName:"XERO",
            secondaryName:"TODO"
        }
    }
}
</script>

<style>
.navbar {
    height: 5rem;
    width: 70%;
    margin: 5rem;
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin: 0 auto;
    /* border: 1px solid red; */
}

.navbar .logo {
    font-size: 3rem;

}

.logo_primary,
.logo_second {
    font-weight: 800;
}

.logo_primary {
    color: #CEBEA4;
}

.logo_second {
    color: #FF5631;
    margin-left: -0.5rem;
}

.navbar .logout {
    color: aliceblue;
}

.logout .fa-right-from-bracket {
    font-size: 2rem;
    color: #CEBEA4;
}
</style>
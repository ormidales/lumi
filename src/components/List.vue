<template>
    <div class="list">
        <div>Première conversation</div>
        <div>
            <span class="documents">Documents</span>
            <small>/</small>
            Première conversation
        </div>
        <div>
            <span class="plaintes">Plaintes</span>
            <small>/</small>
            Première conversation
        </div>
        <div>
            <span class="actions">Actions</span>
            <small>/</small>
            Première conversation
        </div>
        <div>
            <span class="incidents">Incidents</span>
            <small>/</small>
            Première conversation
        </div>
    </div>
</template>

<style scoped>
.list {
    width: 100%;
    display: flex;
    flex-direction: column;
    gap: 8px;
}

.list>div {
    padding: 8px 16px;
    display: flex;
    align-items: center;
    justify-content: flex-start;
    gap: 8px;
    background-color: rgba(50, 50, 50, 0.5);
    border-radius: 8px;
    font-size: 14px;
    color: rgba(255, 255, 255, 0.8);
    cursor: pointer;
    transition: all 0.3s ease-in-out;
}

.list>div:hover {
    background-color: rgba(50, 50, 50, 0.8);
    color: rgba(255, 255, 255, 1);
    transform: scale(1.01);
}

.list>div:active {
    transform: scale(0.99);
}

.list>div>small {
    font-size: 10px !important;
    color: rgba(255, 255, 255, 0.5) !important;
}

.list>div>span {
    padding: 2px 4px;
    border-radius: 4px;
    font-size: 12px !important;
    font-weight: 600;
    color: rgba(255, 255, 255, 1) !important;
}

.list>div>span.documents {
    background-color: rgba(70, 220, 40, 1);
}

.list>div>span.plaintes {
    background-color: rgba(204, 39, 219, 1);
}

.list>div>span.actions {
    background-color: rgba(219, 118, 39, 1);
}

.list>div>span.incidents {
    background-color: rgba(73, 39, 219, 1);
}
</style>

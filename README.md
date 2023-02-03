Обезличенный пример теста со стажировки


<b>@Epic("Спринт 999") <br>
@Feature("Казначейство")<br>
@Story("Настроить Drilldown в Fiori")<br>
@Issue("S4S-7777777")<br>
@Tag("Study")<br>
@SystemSAP(sys = SAPSystems.S4T)<br>
@UserSAP(user = Credentials.DM_TEST)<br>
@ExtendWith(TestSuiteHardAssert.class)<br>
@DisplayName("S4.R2R.Настроить Drilldown в Fiori. Set test ID - 7777")<br><br>

public class Test_CheckAnalysisMoneyMarket extends BaseState {

    @Test
    @URL(url = WebURL.FIORI_DM)
    @DisplayName("FIORI - Анализ денежного рынка. Test ID - 99999")
    void testLookAccount() {
        MainPageFiori mainPage = new LoginPageFiori().logIn(Credentials.DM_TEST);
        CashFlowAnalysisPage page = new CashFlowAnalysisPage();

        step("1. На начальной странице нажать на плитку 'Анализ денежного потока'", () ->
                mainPage.selectCard("Анализ денежного потока"));

        step("2. Заполнить необходимые поля для вывода отчета и нажать кнопку 'Применить'", () -> {
            page.waitForVisibility(page.BE, 90);
            page.sendValue(page.BE, "1000" + Keys.ENTER);
            page.sendValue(page.DATE, "01.01.2000");
            page.sendValue(page.BANK_ACCOUNT, "555556666600000000000");
            page.click(page.APPLY);
            page.waitForVisibility(page.CURRENCY_RUB, 90);
        });

        step("Раскрыть все строки в таблице", page::expandAllRows);

        step("4. В раскрывшейся таблице нажать на нужную ссылку и выбрать в окне действие", () -> {
            page.clickByLinkText("-120.000,00 RUB");
            page.waitForVisibility(page.POPOVER_TITLE, 30);
            page.clickByLinkText("Просмотреть позиции денежного потока");
        });

        step("Проверка заголовка на соответствие выбранной ячейки", () -> {
            page.waitForVisibility(page.SHELL);
            assertEquals("-120.000,00 RUB", page.getHeaderPositionText(2), "Заголовок не соответствует");
        });
    }
}
